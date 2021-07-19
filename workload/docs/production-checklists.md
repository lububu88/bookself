# Production ready checklist
## General
- Logs
  + Logs nên được ghi theo `STDIN`, `STDERR`
  + Kubernetes thực thi log rotation hàng ngày, hoặc sẽ được rotate khi 10MB. Mặc định Kubernetes sẽ giữ 5 file Logs trong 1 container. (có thể điều chỉnh được thông số này)
  + `LOG_LEVEL` nên được chia ra theo dạng: `DEBUG`, `TRACE`, `ERROR`, `WARN`, `INFO` 
  + `https://kubernetes.io/docs/concepts/cluster-administration/logging/`

- Config:
  + Config nên sử dụng là ENV hoặc configmap, nên sử dụng là configmap.

- Application Stateful, Stateless:
  + Hiện tại trong Phase này, team chỉ hỗ trợ ứng dụng Stateless.

## Devops
- *Dockerfile*: locally buildable, runable. Nên sử dụng các Official Image based nếu có thể. 
- *Docker-compose* sử dụng docker-compose để locally tested/runnable 
- Health check: `/health/live` for liveness or readiness. Liveness có nghĩa là service sẽ bị kill nếu nó không trả về responds trong vòng 1s, 2s ...
- Readiness check: `/health/ready` , nghĩa là service đã sẵn sàng để tiếp nhận connection/requests, điều này thường cần đảm bảo rằng không chỉ các cổng phục vụ được mở, mà các phần phụ thuộc khác như kết nối database, redis, rabbitmq cũng sẵn sàng. Nếu /ready check là false, nhưng /health/live check là true, service is up , không restart nó, nhưng sẽ không gửi traffic tới nó nữa.
- Health+ready: `/health`: If (!exists(liveness) && !exists(readiness)), must be implements simple endpoint to serve default healthly check (return 200, respond < 1s)
  + `https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/`
  + `https://viblo.asia/p/kubernetes-best-practices-liveness-va-readiness-health-checks-4dbZN9R8KYM`

- Health-check:
  + /health/live: return 200 OK
  + /health/ready: check if {redis, db, rabbitMQ} is ready, return 200 OK
  
- Graceful shutdown: The application understands SIGTERM and other signals and will gracefully shutdown itself after processing the current task. 
  + `https://kubernetes.io/docs/concepts/workloads/pods/pod/#termination-of-pods`
  + `https://techmaster.vn/posts/34120/graceful-shutdown-voi-nodejs-va-kubernete`
- Metrics ready: `/metrics` , Cung cấp các metrics cơ bản để prometheus có thể scarping, dựa trên cơ sở đó để tối ưu hóa ứng dụng.... 
  + metric custumize
  + `https://tomgregory.com/the-four-types-of-prometheus-metrics/`
- Alerts nên được cấu hình, default standard alert should be set. **RED** ( Rate, Error, Duration), **USE** (Utilization, Saturation, Errors)
  + CPU / memory tăng đột biến.
  + Traffic tăng giảm đột ngột (2xx, 4xx, 5xx)
  + Số lượng transactions processed per second (TPS) cần được theo dõi
- Auto Scale Application for CPU, Mem ... test tải tối ưu hóa tài nguyên. Using ab to test `ab -n 10000 -c 10 https://demoweb-test.vnpaytest.vn/`

- Rollout and Rollback service: Plan Customize: cụ thể chi tiết cho kế hoạch