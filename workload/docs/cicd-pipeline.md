### Basic Flow

![Flow](static/cicd_pipeline.png)

#### Note:

* Workload được quản lý 100% bằng git, HEAD sẽ chứa state hiện tại của workload, bao gồm tất cả config, và service dependencies của nó.
* Dùng gitlab runner để tự provison workload bên trong của từng cluster thông qua helm, helmfile, helm chart
* Mỗi cluster được provison trên gitlab-ci runner tag từng môi trường.
* ROLLBACK bằng git commit tag or helmfile rollout revision.
* Cluster, Application khôi phục thảm họa có thể được thực hiện đơn giản bằng cách deploy lại gitlab-runner, apply infra, rồi apply lại workload