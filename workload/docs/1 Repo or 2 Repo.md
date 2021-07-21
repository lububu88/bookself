### 1 Repo or 2 Repo

----
                    
### Ưu nhược điểm
                    
Fuction  | 1 Repo (CICD)  | 2 Repo (CI and CD)
------------- | ------------- | -------------
Working   | Dev làm việc trên cùng 1 Repo CICD trên Git Department  | Dev làm việc trên cùng 2 Repo: Repo CI và Repo CD. Tách bạch riêng rẽ giữa việc CI và CD.
Manage   | Dev quản lý việc Deployment lên hệ thống K8S một cách chủ động. SRE đưa thông tin connect to Cluster K8S cho Dev  | Dev và SRE cần phối hợp với nhau để đưa dịch vụ lên các môi trường. Sẽ có quy trình Review và Approval, MR cho việc này. Dev sẽ có tài khoản theo RBAC để truy cập Cluster để  TS như view, get, not edit, not delete ...  
Resouce   | Dev chủ động cân đối về Resouce sử dụng cho từng ứng dụng, nếu làm không tốt có thể gây lãng phí tài nguyên  | Dev và SRE cần phối hợp với nhau để đưa ra resouce tối ưu cho ứng dụng, autoscaling (scaleup, scaledown) hợp lý. 
Khi xẩy ra sự cố lớn  |  Do việc phân tác Code CI nằm ở các project khác nhau, nên việc để deploy lại service là lâu hơn và khó kiểm soát hơn.  | Do toàn bộ Workload nằm trên cùng 1 Repo CD, việc rollback lại toàn bộ ứng dụng là đơn giản hơn và tránh được missmatch thông tin.  
Move App Site to Site  |  Rất khó để làm việc này  | Đơn giản để move App Site to Site, Site to Cloud. Chú ý về phần DB Replication nếu muốn sử dụng.            
----



