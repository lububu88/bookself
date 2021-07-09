### How to deploy a service into Cluster K8S

* Application need build and deploy in [dev,test,...] before release on [prod]
* In [dev,test,..] dev full controls cluster, dev integrates CICD on one repo. (action in Gitlab Department)
* In [prod] using images tag running on [test] and pass all case test, dev must using template CD define to Cluster Prod (action in Gitlab System)
* Build docker image (use any, familiar CI)
* Push docker image as following name `hub.vnpaytest.vn/[your_project]/[your-image]:[version]-[short-sha1-hash]`
    * your-project: hub project
    * your-image-name: your image repository
    * version: version application
    * short-sha1-hash: sha1 hash, which identify your latest commit by the build
* Pull request to your application stack in: `https://git.example.com/workloads-[Site-OR-Department]/prod/vnpay.service`, follow this directory structure
```
.
├── helmfile.yaml # definition of helmfile, common repos, etc...
├── this-is-sample-stack
│   ├── helmfile.yaml # definition of list releases unit
│   ├── service-1.yaml
│   ├── service-2.yaml
│   ├── service-1-mysql.yaml
│   ├── service-1-redis.yaml
│   └── service-2-mysql.yaml
├── [your-stack]
│   ├── helmfile.yaml # definition of list services in your stack
│   ├── [your-service-name].yaml # your service config here
```

* Follow `[your-service-name].yaml` must be following structure of charts at `https://hub.vnpaytest.vn/harbor/projects/1/helm-charts`

```
# Sample for generic:1.0.0
nameOverride: ""
fullnameOverride: ""

image:
  repository: hub.vnpaytest.vn/{your-project}/{your-image} # REQUIRED
  tag: [your-tag]          # REQUIRED
  pullPolicy: IfNotPresent
  pullSecrets: []

########## App settings ##########
app:
  kind: Deployment
  replicas: null
  command: null
  args: null
  port: 80
  annotations: {}
  podAnnotations: {}
    ## In order to get prometheus to scrape pods
    # prometheus.io/scrape: "true"
    # prometheus.io/path: /metrics
    # prometheus.io/port: "8080"
    ## In order to fluentbit parse logs
    # fluentbit.io/parser: apache
  healthcheck:
    liveness: {}
    #   periodSeconds: 3
    #   timeoutSeconds: 10
    #   httpGet:
    #     path: "/"
    #     port: http
    readiness: {}
    startup: {}
  schedulerName: null
  runtimeClassName: null
  priorityClassName: null

resources: {}
  # We usually recommend not to specify default resources and to leave this as a conscious
  # choice for the user. This also increases chances charts run on environments with little
  # resources, such as Minikube. If you do want to specify resources, uncomment the following
  # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  # limits:
  #   cpu: 100m
  #   memory: 128Mi
  # requests:
  #   cpu: 100m
  #   memory: 128Mi

env: []
envFrom: []

volumes: []
# - name: configs
#   configMap:
#     name: configs
#     optional: true

volumeMounts: []
# - name: configs
#   mountPath: /etc/spark/configs

volumeDevices: []

configMaps: []
# - name: configs
#   data:
#     foo: bar
#     abc: xyz
#   binaryData:
#     data.bson: binarydata

secrets: []
# - name: credential
#   type: Opaque
#   stringData:
#     username: admin
#   data:
#     password: Zm9vYmFyCg==  # base64 encoded string

podDisruptionBudget:
  enabled: false
  # minAvailable: 1
  # maxUnavailable: 1

autoscaling:
  enabled: false
  min: 1
  max: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 80

########## Placement ##########
nodeSelector: {}
tolerations: []
affinity: {}

########## RBAC, Serivce Account & Security Configs ##########
serviceAccount:
  # Specifies whether a service account should be created
  create: false
  # The name of the service account to use.
  # If not set and create is true, a name is generated using the fullname template
  name:

# rbac:
#   # If true, create & use RBAC resources
#   create: true
#   # If true, create and use PodSecurityPolicy
#   pspEnable: true
#   # The name of the ServiceAccount to use.
#   # If not set and create is true, a name is generated using the fullname template
#   name:

podSecurityContext: {}
  # fsGroup: 2000

# securityContext at container level
securityContext: {}
  # capabilities:
  #   drop:
  #   - ALL
  # readOnlyRootFilesystem: true
  # runAsNonRoot: true
  # runAsUser: 1000

########## Networking ##########
service:
  type: ClusterIP
  port: 80
  annotations: {}
    ## In order to get prometheus to scrape service
    # prometheus.io/scrape: "true"
    # prometheus.io/path: /metrics
    # prometheus.io/port: "8080"

ingress:
  enabled: false
  annotations: {}
    # kubernetes.io/ingress.class: nginx
    # kubernetes.io/tls-acme: "true"
  hosts:
  - host: example.com
    path: /  #### /testpath
    #### README https://kubernetes.io/docs/concepts/services-networking/ingress/
    #### Path types
    # pathType: Prefix  

########## Observation ##########
serviceMonitor:
  enabled: false
  additionalLabels: {}
  interval: 30s
  honorLabels: false
  relabellings: {}
```

* Add a segment to `[your-stack]/helmfile.yaml`

```
releases:
- name: {your-helm-release-name]
  namespace: {your-namespace}
  chart: vnpay/generic
  version: 1.0.0
  values:
  - ./{your-service].yaml
  - fullnameOverride: {your-helm-release-name}
```

* PR and enjoy