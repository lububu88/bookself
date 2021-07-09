# Production ready checklist
## General
- Logs
  + All logs should be written to `STDIN`, `STDERR`
  + Do not write sensitive data ( password ...), ID to log
  + Kubernetes performs log rotation daily, or if the log file grows beyond 10MB in size. Each rotation belongs to a single container; if the container repeatedly fails or the pod is evicted, all previous rotations for the container are lost. By default, Kubernetes keeps up to five logging rotations per container.
  + Standard envvar `LOG_LEVEL` should be used to set different verbosity: `DEBUG`, `TRACE`, `ERROR`, `WARN`, `INFO` 
  + `https://kubernetes.io/docs/concepts/cluster-administration/logging/`

## Devops
- *Dockerfile*: locally buildable, runable. Should be alpine/distroless based if possible. 
- *Docker-compose* file should be available for locally tested/runnable 
- Health check: `/health/live` for liveness or readiness. Liveness, means, service will be killed of it doesnt return result within 1 seconds
- Readiness check: `/health/ready` , means service is ready to accept connection/requests, this normally need to make sure not only serving ports are openned, but other dependencies like database connection is ready as well. A false /ready check, but true /health check generaly means, service is up , dont restart it yet, but do not send traffic to it.
- Health+ready: `/health`: If (!exists(liveness) && !exists(readiness)), must be implements simple endpoint to serve default healthly check (return 200, respond < 1s)
  + `https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/`
  + `https://viblo.asia/p/kubernetes-best-practices-liveness-va-readiness-health-checks-4dbZN9R8KYM`

- Health-check:
  + /health/live: return 200 OK
  + /health/ready: check if {redis, db, rabbitMQ} is ready, return 200 OK
  
- Graceful shutdown: The application understands SIGTERM and other signals and will gracefully shutdown itself after processing the current task. 
  + `https://kubernetes.io/docs/concepts/workloads/pods/pod/#termination-of-pods`
  + `https://techmaster.vn/posts/34120/graceful-shutdown-voi-nodejs-va-kubernete`
- Metrics ready: `/metrics` , serving standard prometheus metrics. 
  + `https://tomgregory.com/the-four-types-of-prometheus-metrics/`
- Alerts should be configured, default standard alert should be set. **RED** ( Rate, Error, Duration), **USE** (Utilization, Saturation, Errors)
  + CPU / memory consumption has increased dramatically.
  + Traffic has increased / fell dramatically.
  + The number of transactions processed per second has brightly changed in any direction.
  + The percentage of errors or their frequency exceeded the permissible threshold (4xx, 5xx)
  + The service stopped sending metrics (a situation often overlooked).
- Rollout and Rollback service: Plan Customize