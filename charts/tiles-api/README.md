# 2GIS Tiles API service

Use this Helm chart to deploy Tiles API service, which is a part of 2GIS's [On-Premise Maps services](https://docs.2gis.com/en/on-premise/map).

Read more about the On-Premise solution [here](https://docs.2gis.com/en/on-premise/overview).

> **Note:**
>
> All On-Premise services are beta, and under development.

See the [documentation](https://docs.2gis.com/en/on-premise/map) to learn about:

- Architecture of the service.

- Installing the service.

    When filling in the keys for `values-tiles.yaml` configuration file, refer to the documentation and the list of keys below.

- Updating the service.

## Values

### Docker Registry settings

| Name                  | Description                                                                             | Value |
| --------------------- | --------------------------------------------------------------------------------------- | ----- |
| `dgctlDockerRegistry` | Docker Registry endpoint where On-Premise services' images reside. Format: `host:port`. | `""`  |


### Deployment Artifacts Storage settings

| Name                     | Description                                                                                                                                                                                                                                              | Value |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- |
| `dgctlStorage.host`      | S3 endpoint. Format: `host:port`.                                                                                                                                                                                                                        | `""`  |
| `dgctlStorage.bucket`    | S3 bucket name.                                                                                                                                                                                                                                          | `""`  |
| `dgctlStorage.accessKey` | S3 access key for accessing the bucket.                                                                                                                                                                                                                  | `""`  |
| `dgctlStorage.secretKey` | S3 secret key for accessing the bucket.                                                                                                                                                                                                                  | `""`  |
| `dgctlStorage.manifest`  | The path to the [manifest file](https://docs.2gis.com/en/on-premise/overview#nav-lvl2@paramCommon_deployment_steps). Format: `manifests/0000000000.json`.<br> This file contains the description of pieces of data that the service requires to operate. | `""`  |


### Tiles API configuration

| Name          | Description                                                                                                                                                                                                                                                                             | Value             |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| `serviceName` | Name of the service.<br>It depends on the [deployment configuration](https://docs.2gis.com/en/on-premise/map#nav-lvl1@paramArchitecture): <ul><li>`tiles-api-webgl` for Tiles API with vector tiles support</li><li>`tiles-api-raster` for Tiles API with raster tiles support</li><ul> | `tiles-api-webgl` |
| `name`        | Name of the deployment.                                                                                                                                                                                                                                                                 | `tiles-api`       |
| `type`        | Type of the [deployment configuration](https://docs.2gis.com/en/on-premise/map#nav-lvl1@paramArchitecture):<ul><li>`web` for Tiles API with vector tiles support</li><li>`raster` for Tiles API with raster tiles support</li><ul>                                                      | `web`             |


### Apache Cassandra Data Storage settings

| Name                                | Description                                                                                                                                                         | Value          |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| `cassandra`                         | **Common settings**                                                                                                                                                 |                |
| `cassandra.environment`             | Environment name (`prod`, `stage`, etc).<br>Support for differently named environments allows hosting multiple Tiles API deployments on a single Cassandra cluster. | `""`           |
| `cassandra.hosts`                   | An array of the one of more IP adresses or hostnames of the Apache Cassandra installation.                                                                          | `[]`           |
| `cassandra.replicaFactor`           | Apache Cassandra [replication factor](https://docs.datastax.com/en/cassandra-oss/3.0/cassandra/architecture/archDataDistributeReplication.html).                    | `3`            |
| `cassandra.consistencyLevelRead`    | Apache Cassandra [read consistency level](https://docs.datastax.com/en/cassandra-oss/3.0/cassandra/dml/dmlConfigConsistency.html#Writeconsistencylevels).           | `LOCAL_QUORUM` |
| `cassandra.consistencyLevelWrite`   | Apache Cassandra [write consistency level](https://docs.datastax.com/en/cassandra-oss/3.0/cassandra/dml/dmlConfigConsistency.html#Readconsistencylevels).           | `LOCAL_QUORUM` |
| `cassandra.keyspace`                | Custom user defined keyspace. If the parameter is set, the database cleaning and maintenance processes are skipped                                                  | `""`           |
| `cassandra.credentials`             | **Credentials for accessing Apache Cassandra**                                                                                                                      |                |
| `cassandra.credentials.user`        | User name to connect to the database.                                                                                                                               | `cassandra`    |
| `cassandra.credentials.password`    | User password to connect to the database.                                                                                                                           | `cassandra`    |
| `cassandra.credentials.jmxUser`     | JMX user name to be used by the Kubernetes Importer Job's cleaner process.                                                                                          | `cassandra`    |
| `cassandra.credentials.jmxPassword` | JMX password to be used by the Kubernetes Importer Job's cleaner process.                                                                                           | `cassandra`    |


### API Keys proxy settings

| Name                              | Description                                                                                                                   | Value                             |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| `proxy`                           | **Common settings**                                                                                                           |                                   |
| `proxy.containerPort`             | Port the proxy listens on.                                                                                                    | `5000`                            |
| `proxy.timeout`                   | Proxy timeout, in seconds.                                                                                                    | `5s`                              |
| `proxy.resources`                 | **Kubernetes [resource management settings](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)** |                                   |
| `proxy.resources.requests.cpu`    | A CPU request.                                                                                                                | `50m`                             |
| `proxy.resources.requests.memory` | A memory request.                                                                                                             | `128Mi`                           |
| `proxy.resources.limits.cpu`      | A CPU limit.                                                                                                                  | `1`                               |
| `proxy.resources.limits.memory`   | A memory limit.                                                                                                               | `512Mi`                           |
| `proxy.image`                     | **Docker image settings**                                                                                                     |                                   |
| `proxy.image.repository`          | Docker Repository.                                                                                                            | `2gis-on-premise/tiles-api-proxy` |
| `proxy.image.tag`                 | Docker image tag.                                                                                                             | `v4.22.0`                         |
| `proxy.image.pullPolicy`          | Kubernetes pull policy for the service's Docker image.                                                                        | `IfNotPresent`                    |
| `proxy.access`                    | **API Keys service access settings**                                                                                          |                                   |
| `proxy.access.enabled`            | If access to the [API Keys service](https://docs.2gis.com/en/on-premise/keys) is enabled.                                     | `false`                           |
| `proxy.access.host`               | API Keys endpoint hostname.                                                                                                   | `http://keys-api.host`            |
| `proxy.access.token`              | Service key for Keys API.                                                                                                     | `""`                              |
| `proxy.access.syncPeriod`         | Proxy sync period.                                                                                                            | `2m`                              |


### Tiles API settings

| Name                                        | Description                                                                                                                                                                                              | Value                       |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| `api`                                       | **Common settings**                                                                                                                                                                                      |                             |
| `api.terminationGracePeriodSeconds`         | Duration in seconds the Tiles API service pod needs to terminate gracefully.                                                                                                                             | `30`                        |
| `api.containerPort`                         | Tiles API container port.                                                                                                                                                                                | `8000`                      |
| `api.labels`                                | Kubernetes [labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/).                                                                                                          | `{}`                        |
| `api.annotations`                           | Kubernetes [annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/).                                                                                                | `{}`                        |
| `api.podAnnotations`                        | Kubernetes [pod annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/).                                                                                            | `{}`                        |
| `api.podLabels`                             | Kubernetes [pod labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/).                                                                                                      | `{}`                        |
| `api.nodeSelector`                          | Kubernetes [node selectors](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector).                                                                                      | `{}`                        |
| `api.affinity`                              | Kubernetes pod [affinity settings](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity).                                                                              | `{}`                        |
| `api.tolerations`                           | Kubernetes [tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/) settings.                                                                                        | `{}`                        |
| `api.replicaCount`                          | A replica count for the pod.                                                                                                                                                                             | `3`                         |
| `api.revisionHistory`                       | Revision history limit (used for [rolling back](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) a deployment).                                                           | `1`                         |
| `api.resources`                             | **Kubernetes [resource management settings](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)**                                                                            |                             |
| `api.resources.requests.cpu`                | A CPU request.                                                                                                                                                                                           | `50m`                       |
| `api.resources.requests.memory`             | A memory request.                                                                                                                                                                                        | `128Mi`                     |
| `api.resources.limits.cpu`                  | A CPU limit.                                                                                                                                                                                             | `1`                         |
| `api.resources.limits.memory`               | A memory limit.                                                                                                                                                                                          | `512Mi`                     |
| `api.image`                                 | **Docker image settings**                                                                                                                                                                                |                             |
| `api.image.repository`                      | Docker Repository.                                                                                                                                                                                       | `2gis-on-premise/tiles-api` |
| `api.image.tag`                             | Docker image tag.                                                                                                                                                                                        | `v4.22.0`                   |
| `api.image.pullPolicy`                      | Kubernetes pull policy for the service's Docker image.                                                                                                                                                   | `IfNotPresent`              |
| `api.imagePullSecrets`                      | Kubernetes image pull secrets.                                                                                                                                                                           | `[]`                        |
| `api.strategy.rollingUpdate`                | **Service's Rolling Update strategy settings**                                                                                                                                                           |                             |
| `api.strategy.rollingUpdate.maxUnavailable` | Maximum number of pods that can be created over the desired number of pods when doing [rolling update](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment). | `0`                         |
| `api.strategy.rollingUpdate.maxSurge`       | Maximum number of pods that can be unavailable during the [rolling update](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment) process.                     | `1`                         |
| `api.service`                               | **Kubernetes [service settings](https://kubernetes.io/docs/concepts/services-networking/service/) to expose the service**                                                                                |                             |
| `api.service.port`                          | Service port.                                                                                                                                                                                            | `80`                        |
| `api.service.type`                          | Kubernetes [service type](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types).                                                                           | `ClusterIP`                 |
| `api.service.annotations`                   | Kubernetes [service annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/).                                                                                        | `{}`                        |
| `api.service.labels`                        | Kubernetes [service labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/).                                                                                                  | `{}`                        |


### Kubernetes [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) settings

| Name                        | Description                            | Value            |
| --------------------------- | -------------------------------------- | ---------------- |
| `api.ingress.enabled`       | If Ingress is enabled for the service. | `false`          |
| `api.ingress.hosts[0].host` | Hostname for the Ingress service.      | `tiles-api.host` |


### Kubernetes [pod disruption budget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/#pod-disruption-budgets) settings

| Name                     | Description                                          | Value  |
| ------------------------ | ---------------------------------------------------- | ------ |
| `api.pdb.enabled`        | If PDB is enabled for the service.                   | `true` |
| `api.pdb.minAvailable`   | How many pods must be available after the eviction.  | `""`   |
| `api.pdb.maxUnavailable` | How many pods can be unavailable after the eviction. | `1`    |


### Kubernetes [Horizontal Pod Autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) settings

| Name                                          | Description                                                                                                                                                          | Value   |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `api.hpa.enabled`                             | If HPA is enabled for the service.                                                                                                                                   | `false` |
| `api.hpa.minReplicas`                         | Lower limit for the number of replicas to which the autoscaler can scale down.                                                                                       | `1`     |
| `api.hpa.maxReplicas`                         | Upper limit for the number of replicas to which the autoscaler can scale up.                                                                                         | `1`     |
| `api.hpa.scaleDownStabilizationWindowSeconds` | Scale-down window.                                                                                                                                                   | `""`    |
| `api.hpa.scaleUpStabilizationWindowSeconds`   | Scale-up window.                                                                                                                                                     | `""`    |
| `api.hpa.targetCPUUtilizationPercentage`      | Target average CPU utilization (represented as a percentage of requested CPU) over all the pods; if not specified the default autoscaling policy will be used.       | `50`    |
| `api.hpa.targetMemoryUtilizationPercentage`   | Target average memory utilization (represented as a percentage of requested memory) over all the pods; if not specified the default autoscaling policy will be used. | `""`    |


### Kubernetes [Vertical Pod Autoscaling](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/README.md) settings

| Name                        | Description                                                                                                  | Value   |
| --------------------------- | ------------------------------------------------------------------------------------------------------------ | ------- |
| `api.vpa.enabled`           | If VPA is enabled for the service.                                                                           | `false` |
| `api.vpa.updateMode`        | VPA [update mode](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler#quick-start). | `Auto`  |
| `api.vpa.minAllowed.cpu`    | Lower limit for the number of CPUs to which the autoscaler can scale down.                                   | `100m`  |
| `api.vpa.minAllowed.memory` | Lower limit for the RAM size to which the autoscaler can scale down.                                         | `128Mi` |
| `api.vpa.maxAllowed.cpu`    | Upper limit for the number of CPUs to which the autoscaler can scale up.                                     | `1`     |
| `api.vpa.maxAllowed.memory` | Upper limit for the RAM size to which the autoscaler can scale up.                                           | `512Mi` |


### Kubernetes Importer job settings

| Name                                         | Description                                                                                                                                                                                                                                                                                                                          | Value                                |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------ |
| `importer`                                   | **Common settings**                                                                                                                                                                                                                                                                                                                  |                                      |
| `importer.enabled`                           | If Importer job is enabled.                                                                                                                                                                                                                                                                                                          | `true`                               |
| `importer.workerNum`                         | Number of parallel import processes (workers).                                                                                                                                                                                                                                                                                       | `6`                                  |
| `importer.writerNum`                         | Number of write processes per import process (worker).                                                                                                                                                                                                                                                                               | `8`                                  |
| `importer.tolerations`                       | Kubernetes [tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/) settings.                                                                                                                                                                                                                    | `{}`                                 |
| `importer.nodeSelector`                      | Kubernetes [node selectors](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector).                                                                                                                                                                                                                  | `{}`                                 |
| `importer.resources`                         | **Kubernetes [resource management settings](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)**                                                                                                                                                                                                        |                                      |
| `importer.resources.requests.cpu`            | A CPU request.                                                                                                                                                                                                                                                                                                                       | `50m`                                |
| `importer.resources.requests.memory`         | A memory request.                                                                                                                                                                                                                                                                                                                    | `128Mi`                              |
| `importer.resources.limits.cpu`              | A CPU limit.                                                                                                                                                                                                                                                                                                                         | `100m`                               |
| `importer.resources.limits.memory`           | A memory limit.                                                                                                                                                                                                                                                                                                                      | `256Mi`                              |
| `importer.image`                             | **Docker image settings**                                                                                                                                                                                                                                                                                                            |                                      |
| `importer.image.repository`                  | Docker Repository.                                                                                                                                                                                                                                                                                                                   | `2gis-on-premise/tiles-api-importer` |
| `importer.image.tag`                         | Docker image tag.                                                                                                                                                                                                                                                                                                                    | `v4.22.0`                            |
| `importer.image.pullPolicy`                  | Kubernetes pull policy for the service's Docker image.                                                                                                                                                                                                                                                                               | `IfNotPresent`                       |
| `importer.imagePullSecrets`                  | Kubernetes image pull secrets.                                                                                                                                                                                                                                                                                                       | `[]`                                 |
| `importer.cleaner`                           | **Cassandra keyspace lifecycle management and Cleaner settings**                                                                                                                                                                                                                                                                     |                                      |
| `importer.forceImport`                       | If enabled, then the Importer job will delete existing keyspace and do import, otherwise import will be skipped.                                                                                                                                                                                                                     | `false`                              |
| `importer.clearSnapshots`                    | If enabled, then the Importer job will delete keyspace's snapshot as well when deleting a keyspace.<br>It executes the `nodetool clearsnapshot` command over JMX to do so, and therefore requires JMS to be enabled on the Cassandra side, and `cassandra.credentials.jmxUser`/`cassandra.credentials.jmxPassword` values to be set. | `false`                              |
| `importer.cassandraHostsClockTimeCheckLimit` | Maximum difference over cassandra hosts clock time.                                                                                                                                                                                                                                                                                  | `1s`                                 |
| `importer.cleaner.enabled`                   | Enables deletion of obsolete tilesets before making new imports.                                                                                                                                                                                                                                                                     | `false`                              |
| `importer.cleaner.limit`                     | Limit on the number of old tilesets to leave untouched when cleaning, minimum 1.                                                                                                                                                                                                                                                     | `3`                                  |
| `importer.workerResources`                   | **Kubernetes [resource management settings](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) for the cleaner process**                                                                                                                                                                                |                                      |
| `importer.cleaner.resources.requests.cpu`    | A CPU request.                                                                                                                                                                                                                                                                                                                       | `50m`                                |
| `importer.cleaner.resources.requests.memory` | A memory request.                                                                                                                                                                                                                                                                                                                    | `128Mi`                              |
| `importer.cleaner.resources.limits.cpu`      | A CPU limit.                                                                                                                                                                                                                                                                                                                         | `1000m`                              |
| `importer.cleaner.resources.limits.memory`   | A memory limit.                                                                                                                                                                                                                                                                                                                      | `512Mi`                              |
| `importer.workerResources`                   | **Kubernetes [resource management settings](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) for the workers**                                                                                                                                                                                        |                                      |
| `importer.workerResources.requests.cpu`      | A CPU request.                                                                                                                                                                                                                                                                                                                       | `256m`                               |
| `importer.workerResources.requests.memory`   | A memory request.                                                                                                                                                                                                                                                                                                                    | `512Mi`                              |
| `importer.workerResources.limits.cpu`        | A CPU limit.                                                                                                                                                                                                                                                                                                                         | `2`                                  |
| `importer.workerResources.limits.memory`     | A memory limit.                                                                                                                                                                                                                                                                                                                      | `2048Mi`                             |


## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| 2gis | <on-premise@2gis.com> | <https://github.com/2gis> |
