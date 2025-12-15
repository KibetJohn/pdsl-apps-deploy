# PDSL App Helm Chart

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release -f sample-values.yaml ./pdsl-app
```

## Configuration

The following table lists the configurable parameters of the pdsl-app chart and their default values.

### Common

| Parameter          | Description                                                                                                                  | Required | Default  |
|--------------------|------------------------------------------------------------------------------------------------------------------------------|----------|----------|
| `Name`             | `pdsl-app.name`, see [_helpers.tpl](templates/_helpers.tpl) for more information.                                            | **true** | `N/A`    |
| `nameOverride`     | Overrides `pdsl-app.name` definition value, see [_helpers.tpl](templates/_helpers.tpl) for more information.                 | false    | `N/A`    |
| `fullnameOverride` | Overrides `pdsl-app.fullname` definition value, see [_helpers.tpl](templates/_helpers.tpl) for more information.             | false    | `N/A`    |
| `image.repository` | The repository to pull container image.                                                                                      | **true** | `N/A`    |
| `image.tag`        | The image tag to pull and run.                                                                                               | **true** | `N/A`    |
| `image.pullPolicy` | Container image pull policy.                                                                                                 | false    | `Always` |
| `image.secrets`    | List of secrets to enable pull images from a private registry.                                                               | false    | `{}`     |
| `containerPort`    | Port number used to access container's running service.                                                                      | **true** | `N/A`    |
| `env`              | Environment variables definitions.                                                                                           | false    | `{}`     |
| `secretEnv`        | Environment variables definitions mapped to [Kubernetes secrets](https://kubernetes.io/docs/concepts/configuration/secret/). | false    | `{}`     |
| `java_opts`        | Sets `JAVA_OPTS` environment variable.                                                                                       | false    | `N/A`    |

### Data & Storage

| Parameter                       | Description                                                                                                                                                                                                 | Required | Default         |
|---------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-----------------|
| `persistence.enabled`           | Claim a persistent volume with properties specified under the `persistence` root property.                                                                                                                  | false    | `false`         |
| `persistence.accessMode`        | The desired access mode the volume should have, see [persistent-volumes access-modes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes) for more information.                   | false    | `ReadWriteOnce` |
| `persistence.size`              | Amount of storage to request.                                                                                                                                                                               | false    | `1Gi`           |
| `persistence.storageClass`      | If no `persistence.existingClaim` property is set, this would be the storage class used for the newly created persistent volume.                                                                            | false    | `"-"`           |
| `persistence.existingClaim`     | Name of the existing persistence volume claim to use, if not specified a new persistent volume claim with name `<pdsl-app.fullname>` gets created, see [pvc.yaml](templates/pvc.yaml) for more information. | false    | `N/A`           |
| `persistence.name`              | Name associated to the persistent volume claim mapping within the pod's spec.                                                                                                                               | **true** | `N/A`           |
| `persistence.mounts`            | List of mount configurations, see [Volume Mount](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#volumemount-v1-core) for more information.                                            | false    | `[]`            |
| `persistenceData.enabled`       | Claim a persistent volume with properties specified under the `persistenceData` root property.                                                                                                              | false    | `false`         |
| `persistenceData.name`          | Name associated to the persistent volume claim mapping within the pod's spec.                                                                                                                               | **true** | `N/A`           |
| `persistenceData.existingClaim` | Name of the existing persistence volume claim to use.                                                                                                                                                       | false    | `false`         |
| `persistenceData.mounts`        | List of mount configurations, see [Volume Mount](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#volumemount-v1-core) for more information.                                            | false    | `false`         |
| `configmapVolumes`              | List of config map volumes configurations containing the properties: `name`, `mountPath`, and `subPath`.                                                                                                    | false    | `[]`            |
| `secretVolumes`                 | List of secret volumes configurations containing the properties: `name`, `mountPath`, and `subPath`.                                                                                                        | false    | `[]`            |
| `application.enabled`           | Enables DB migration job, see [db-migrate-hook.yaml](templates/db-migrate-hook.yaml).                                                                                                                       | false    | `false`         |
| `flyway`                        | Enables flyway migration job, see [db-flyway-migration.yaml](templates/db-flyway-migration.yaml).                                                                                                           | false    | `{}`            |

### Health Checks

| Parameter                            | Description                                                                                | Required | Default |
|--------------------------------------|--------------------------------------------------------------------------------------------|----------|---------|
| `livenessProbe.path`                 | Health check endpoint path.                                                                | **true** | `N/A`   |
| `livenessProbe.port`                 | Health check port number.                                                                  | **true** | `N/A`   |
| `livenessProbe.initialDelaySeconds`  | Delay before liveness probe is initiated.                                                  | false    | `30`    |
| `livenessProbe.periodSeconds`        | How often to perform the probe.                                                            | false    | `10`    |
| `livenessProbe.timeoutSeconds`       | When the probe times out.                                                                  | false    | `5`     |
| `livenessProbe.failureThreshold`     | Minimum consecutive failures for the probe to be considered failed after having succeeded. | false    | `6`     |
| `readinessProbe.path`                | Health check endpoint path.                                                                | **true** | `N/A`   |
| `readinessProbe.port`                | Health check port number.                                                                  | **true** | `N/A`   |
| `readinessProbe.initialDelaySeconds` | Delay before readiness probe is initiated.                                                 | false    | `5`     |
| `readinessProbe.periodSeconds`       | How often to perform the probe.                                                            | false    | `10`    |
| `readinessProbe.timeoutSeconds`      | When the probe times out.                                                                  | false    | `5`     |
| `cliHealthCheck.enabled`             | Enable CLI health check commands.                                                          | false    | `false` |
| `cliHealthCheck.command`             | List of commands to run for health check.                                                  | false    | `[]`    |

### Observability

| Parameter                    | Description                                                                                                                          | Required | Default |
|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|----------|---------|
| `opentelemetry.enabled`      | Enable OpenTelemetry agent injection.                                                                                                | false    | `false` |
| `opentelemetry.injectJava`   | Enable OpenTelemetry Java Injection Instrumentation.                                                                                 | false    | `true`  |
| `opentelemetry.injectPython` | Enable OpenTelemetry Python Injection Instrumentation.                                                                               | false    | `true`  |
| `opentelemetry.injectNodejs` | Enable OpenTelemetry NodeJS Injection Instrumentation.                                                                               | false    | `true`  |
| `prometheus.enabled`         | **(deprecated)** serviceMonitor to be scraped in prometheus-operator.                                                                | false    | `false` |
| `prometheus.interval`        | **(deprecated)** Default frequency to scrap metrics.                                                                                 | false    | `15s`   |
| `AlertsLists`                | **(deprecated)** To add list of Prometheus Rules alerting, see [prometheus-alert-rules.yaml](templates/prometheus-alert-rules.yaml). | false    | `[]`    |
| `Alerts`                     | **(deprecated)** To add alerting rules, see [prometheus-alert-rules.yaml](templates/prometheus-alert-rules.yaml).                    | false    | `{}`    |

### Network Access & Security

| Parameter                                  | Description                                                                                                                                                                                                                                                                                                                                       | Required | Default                        |
|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|--------------------------------|
| `securityContext.enabled`                  | Enable security context customization, see [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/) and [securitycontext-v1-core](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#securitycontext-v1-core) for more information.                                                       | false    | `true`                         |
| `securityContext.fsGroup`                  | Group ID for the container.                                                                                                                                                                                                                                                                                                                       | **true** | `N/A`                          |
| `securityContext.runAsUser`                | User ID for the container.                                                                                                                                                                                                                                                                                                                        | **true** | `N/A`                          |
| `securityContext.runAsNonRoot`             | Indicates that the container must run as a non-root user.                                                                                                                                                                                                                                                                                         | false    | `true`                         |
| `securityContext.capabilities`             | The capabilities to add/drop when running containers, see [capabilities-v1-core](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#capabilities-v1-core) for more information.                                                                                                                                                 | false    | `{}`                           |
| `securityContext.allowPrivilegeEscalation` | Whether a process can gain more privileges than its parent process.                                                                                                                                                                                                                                                                               | false    | `false`                        |
| `securityContext.readOnlyRootFilesystem`   | Mounts the container's root filesystem as read-only.                                                                                                                                                                                                                                                                                              | false    | `true`                         |
| `iamRole`                                  | AWS IAM roles to attach to the deployment pods.                                                                                                                                                                                                                                                                                                   | false    | `[]`                           |
| `serviceAccount.create`                    | Create Kubernetes service account.                                                                                                                                                                                                                                                                                                                | false    | `false`                        |
| `serviceAccount.rbac`                      | Kubernetes RBAC permissions tied to service account.                                                                                                                                                                                                                                                                                              | false    | see [values.yaml](values.yaml) |
| `service.type`                             | Service type associated to deployment's default service, see [service.yaml](templates/service.yaml) and [service types](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types) for more information.                                                                                                 | false    | `ClusterIP`                    |
| `service.port`                             | Port number for deployment's default service, see [service.yaml](templates/service.yaml) for more information.                                                                                                                                                                                                                                    | **true** | `N/A`                          |
| `service.admin`                            | Enables admin access to the service via a specific port number.                                                                                                                                                                                                                                                                                   | false    | `false`                        |
| `adminPort`                                | The port number to use for admin access if `service.admin` is enabled                                                                                                                                                                                                                                                                             | **true** | `N/A`                          |
| `ingress.enabled`                          | Enables ingress (external access to the service), see [ingress.yaml](templates/ingress.yaml), [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/), and [ingress-v1-networking-k8s-io](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#ingress-v1-networking-k8s-io) for more information. | false    | `false`                        |
| `ingress.port`                             | Ingress port number.                                                                                                                                                                                                                                                                                                                              | **true** | `N/A`                          |
| `ingress.annotations`                      | Custom [annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) for ingress.                                                                                                                                                                                                                                 | false    | `{}`                           |
| `ingress.tls`                              | TLS configuration, see  [ingresstls-v1-networking-k8s-io](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#ingresstls-v1-networking-k8s-io) for more information.                                                                                                                                                             | false    | `{}`                           |
| `ingress.hosts`                            | List of fully qualified domain name of network hosts, as defined by RFC 3986, see  [ingresstls-v1-networking-k8s-io](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#ingresstls-v1-networking-k8s-io) for more information.                                                                                                  | false    | `[]`                           |
| `internal_endpoint`                        | Object property; if present, enables creation of [internal_service.yaml](templates/internal_service.yaml), (useful in the case `service.type` is set to an external type like `LoadBalancer`)                                                                                                                                                     | false    | `{}`                           |
| `internal_endpoint.name`                   | Name of internal service.                                                                                                                                                                                                                                                                                                                         | **true** | `N/A`                          |
| `internal_endpoint.port`                   | Port number for internal service.                                                                                                                                                                                                                                                                                                                 | **true** | `N/A`                          |
| `internal_endpoint.annotations`            | Custom [annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) for internal service.                                                                                                                                                                                                                        | false    | `{}`                           |
| `internal_endpoint.type`                   | Kubernetes service type, see [service types](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types) for more information.                                                                                                                                                                            | false    | `ClusterIP`                    |

### Scalability & Stability

| Parameter                                       | Description                                                                                                                                                                                                                                                                                                                                                                                                   | Required | Default           |
|-------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| `antiAffinity`                                  | To control podAntiAffinity, see [affinity-and-anti-affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity).                                                                                                                                                                                                                                            | false    | `soft`            |
| `nodeAffinity`                                  | To assign pods to specific nodes, see [affinity-and-anti-affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity).                                                                                                                                                                                                                                      | false    | `{}`              |
| `terminationGracePeriodSeconds`                 | Time needed for container to be gracefully stopped.                                                                                                                                                                                                                                                                                                                                                           | false    | `30`              |
| `hpav2.enabled`                                 | Enables HorizontalPodAutoscaler V2 resource if conditions are met, see [hpa.yaml](templates/hpa.yaml) and [K8s HorizontalPodAutoscaler docs](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) for more information.                                                                                                                                                                | false    | `false`           |
| `replicaCount`                                  | Amount of pods for Deployment's ReplicaSet and HorizontalPodAutoscaler minReplicas value.                                                                                                                                                                                                                                                                                                                     | false    | `1`               |
| `maxReplicaCount`                               | Max amount of pods at any given time.                                                                                                                                                                                                                                                                                                                                                                         | false    |                   |
| `maxUnavailablePods`                            | An eviction is allowed if at most "maxUnavailable" pods selected by "selector" are unavailable after the eviction, i.e. even in absence of the evicted pod, see [poddisruptionbudget-v1-policy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#poddisruptionbudget-v1-policy) for more information.                                                                                     | false    | `25%`             |
| `scalingBehaviour`                              | Enables scaling behavior customization via `scaleDown` and  `scaleUp` policies, see [Scaling policies](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/#scaling-policies) and [horizontalpodautoscalerbehavior-v2beta2-autoscaling](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#horizontalpodautoscalerbehavior-v2beta2-autoscaling) for more information. | false    | `false`           |
| `scaleDown.stabilizationWindowSeconds`          | The number of seconds for which past recommendations should be considered while scaling down.                                                                                                                                                                                                                                                                                                                 | false    | `300`             |
| `scaleDown.podPercent`                          | Percentage of metrics to start scaling down.                                                                                                                                                                                                                                                                                                                                                                  | **true** | `N/A`             |
| `scaleDown.podsCount`                           | Number of pods to start scale down policy.                                                                                                                                                                                                                                                                                                                                                                    | **true** | `N/A`             |
| `scaleDown.periodSeconds`                       | The window of time for which the policy to scale down should hold true.                                                                                                                                                                                                                                                                                                                                       | **true** | `N/A`             |
| `scaleDown.selectPolicy`                        | Overrides which policy to use for scaling down, by default the policy which allows the highest amount of change is the policy which is selected.                                                                                                                                                                                                                                                              | false    | `MaxPolicySelect` |
| `scaleUp.stabilizationWindowSeconds`            | The number of seconds for which past recommendations should be considered while scaling up.                                                                                                                                                                                                                                                                                                                   | false    | `0`               |
| `scaleUp.podPercent`                            | Percentage of metrics to start scaling up.                                                                                                                                                                                                                                                                                                                                                                    | **true** | `N/A`             |
| `scaleUp.podsCount`                             | Number of pods to start scale up policy.                                                                                                                                                                                                                                                                                                                                                                      | **true** | `N/A`             |
| `scaleUp.periodSeconds`                         | The window of time for which the policy to scale up should hold true.                                                                                                                                                                                                                                                                                                                                         | **true** | `N/A`             |
| `scaleUp.selectPolicy`                          | Overrides which policy to use for scaling up, by default the policy which allows the highest amount of change is the policy which is selected.                                                                                                                                                                                                                                                                | false    | `MaxPolicySelect` |
| `scalingMetricsMemory.targetMemoryUtilization`  | Average memory utilization target (only available with `hpav2.enabled`).                                                                                                                                                                                                                                                                                                                                      | **true** | `N/A`             |
| `scalingMetrics.targetCPUUtilizationPercentage` | Average CPU utilization target.                                                                                                                                                                                                                                                                                                                                                                               | **true** | `N/A`             |

### Sidecar & Init Containers

| Parameter                            | Description | Required | Default |
|--------------------------------------|-------------|----------|---------|
| `sidecarContainers`                  |             | false    | `{}`    |
| `sidecarContainers.image.name`       |             | false    | `{}`    |
| `sidecarContainers.image.repository` |             | false    | `{}`    |
| `sidecarContainers.image.tag`        |             | false    | `{}`    |
| `sidecarContainers.image.pullPolicy` |             | false    | `{}`    |
| `sidecarContainers.securityContext`  |             | false    | `{}`    |
| `sidecarContainers.containerPort`    |             | false    | `{}`    |
| `sidecarContainers.extraTCPPorts`    |             | false    | `{}`    |
| `sidecarContainers.httpHealthCheck`  |             | false    | `{}`    |
| `sidecarContainers.livenessProbe`    |             | false    | `{}`    |
| `sidecarContainers.readinessProbe`   |             | false    | `{}`    |
| `sidecarContainers.persistence`      |             | false    | `{}`    |
| `sidecarContainers.configmapVolumes` |             | false    | `{}`    |
| `sidecarContainers.secretVolumes`    |             | false    | `{}`    |
| `sidecarContainers.env`              |             | false    | `{}`    |
| `sidecarContainers.secretEnv`        |             | false    | `{}`    |
| `sidecarContainers.cliHealthCheck`   |             | false    | `{}`    |
| `initContainers`                     |             | false    | `{}`    |

# Boilerplate helm value files:

### Below there is a sample dev environment helm values file. NOTE: Is good practice to keep properties shared by all environments in a common.yaml file such as healthchecks, ports, etc.

common.yaml:
```
# Healthchecks
livenessProbe:
  path: /health # add the path configured in the app, few microservice use /healthcheck as the path
  port: 9000
  initialDelaySeconds: 60
  periodSeconds: 15
  timeoutSeconds: 15
 
readinessProbe:
  path: /health
  port: 9000
  initialDelaySeconds: 60
  periodSeconds: 15
  timeoutSeconds: 15
 
#Disable PVC Documents Mount
persistenceDss:
  enabled: false
 
# Container
containerPort: 9000
adminPort: 9000
 
cliHealthCheck:
  enabled: false
 
# Docker specifics
image:
  repository: 004384079765.dkr.ecr.us-west-2.amazonaws.com/appname # Add the application name which is used in the Jenkinsfile to build the docker image and to push to ECR
  tag: latest
  pullPolicy: IfNotPresent
 
#securityContext
securityContext:
  enabled: true # when you enable true, you cannot do any write operation on the pod when accessing via rancher or using cli. 
  allowPrivilegeEscalation: false
  fsGroup: 10001
  runAsUser: 10001
  runAsNonRoot: true
  readOnlyRootFilesystem: true
 
nodeSelector:

  # Use the following key-value pair to schedule a pod on EKS nodes on all markets.
  
  node.kubernetes.io/role: node  
  
tolerations: []
 
affinity: {}

antiAffinity: soft # schedules a pod on a different node when min number of pod is set as 2 for high availability. When one node goes down, pod running on another node will receive the traffic and will avoid any timeouts
 
# Ingress endpoints
ingress:
  enabled: false
  annotations: {}
  path: /
  hosts:
    - appname.local
  tls: []
 
service:
  type: ClusterIP
  port: 8080
 
# Resources requirements
resources:
  requests: 
    cpu: 100m # Minimum amount of CPU resource required to be allocated at the time of scheduling. Always set this number to match with the resource usage during the peak time in specific region.
    memory: 500Mi # Minimum amount of Memory(RAM) resource required to be allocated at the time of scheduling. Always set this number to match with the resource usage during the peak time in specific region.
  limits:
    cpu: 200m # If node has resources available it will make use of it, if not the HPA will kick in when targetCPUUtilizationPercentage and targetMemoryUtilization reaches 90% usage and HPA will scale up the number of pods.
    memory: 900Mi #
 
scalingMetrics:
  targetCPUUtilizationPercentage: 90 # When reached 90% of usage, HPA will scale the desired number of pods based on the CPU utilization

scalingMetricsMemory:
  targetMemoryUtilization: 90 # When reached 90% of usage, HPA will scale the desired number of pods based on the Memory utilization
  
scaleDown:
  stabilizationWindowSeconds: 900 
  periodSeconds: 15
  podsCount: 1

scaleUp:
  stabilizationWindowSeconds: 0
  periodSeconds: 15
  podsCount: 1

# HPA API v2
hpav2:
  enabled: true
 
# COMMON APPLICATION ENVIRONMENT VARIABLES EXAMPLE (if needed)
env:
  EXAMPLE_ENV_VARIABLE_1: false
  EXAMPLE_ENV_VARIABLE_2: "yes"
  ENABLE_HEALTH_CHECKS: "yes"
```
dev.yaml

```
nameOverride: "dev-appname-service"
fullnameOverride: "dev-appname-service"
 
# PVC Claim Mount Certs
persistence:
  name: dev-certs-efs
  enabled: true
  existingClaim: dev-certs-efs
  accessMode: ReadWriteOnce
  size: 1Gi
  mounts:
    - mountPath: /datadir/
      name: dev-certs-efs
 
# GlusterFS mount
GlusterFS:
  enabled: false
 
 
# Scalability and Stability. If replica count and max ReplicaCount is set same, HPA won't be deployed, it looks for min and max value to be different
replicaCount: 2
maxReplicaCount: 2
 
# Resources requirements
# Only add the below block if you want to override the values from common.yaml for CPU and Memory. 
# Usually for non-prod env usage will be less compare to prod. It is recommended to have a dedicated resource requeust block in prod.yaml helm file.
# Please be careful on setting up this, a proper capacity planning has to be done before setting the values for CPU and Memory. Check the Sumo Logic and see the usage and then set the value. 
# If questions reachout to DevOps. If not sure about resource utilization in prod, deploy the service all the way to prod and then monitor it for 1- 2 weeks on Sumo Logic and get the max CPU and Memory usage.
# After that add 200Mi to Memory and 200m to CPU
resources:
  requests:
    cpu: 400m
    memory: 400Mi
  limits:
    cpu: 600m
    memory: 600Mi
 
maxUnavailablePods: 25%
 
#iamRole 
iamRole: arn:aws:iam::112991787507:role/k8s-dev-appname # Please update account number for each env. For account number refer this Wiki for market and env specific -> https://pdslmobile.atlassian.net/wiki/spaces/DEV/pages/2331803683/PDSL+Infrastructure+Subnets
 
# Environment
environmentShort: dev
environmentFull: development
 
env:
  EXAMPLE_ENV_VARIABLE_1: false
  EXAMPLE_ENV_VARIABLE_2: "yes"
  ENABLE_HEALTH_CHECKS: "yes
 
secretEnv:
  DATABASE_PASSWORD:
    name: appname-mariadb-secret
    key: password
  DATABASE_USERNAME:
    name: appname-mariadb-secret
    key: username
```
