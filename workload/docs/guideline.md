# # Introduction
# # # Tools:
- helmfile: 
- helmv3
- docker hub:
[-] hub.vnpaytest.vn
- cicd platform: gitlabCI
- 

# # # Domain rules
*Webapps:* Truy cập từ enduser.

[dev,test]
- Naming conversion: xxx-dev.vnpaytest.vn, xxx-test.vnpaytest.vn

  + SSL Proxy: offload cert SSL trên đầu Citrix, Nginx, Haproxy -> Proxy tới Ingress-Nginx Cluster Port 80/443

[prod]
- Naming conversion: xxx.vnpay.vn

  + SSL Proxy: offload cert SSL trên đầu Citrix, Nginx, Haproxy -> Proxy tới Ingress-Nginx Cluster Port 80/443


*API Endpoints:* Access from 3rd API (public API)

[dev,test]
- Naming conversion: xxx-dev.vnpayapis.com, xxx-test.vnpayapis.com

   + SSL Proxy: offload cert SSL trên đầu Citrix, Nginx, Haproxy -> Proxy tới Ingress-Nginx Cluster Port 80/443

[prod]
- Naming conversion: xxx.vnpayapis.com

   + SSL Proxy: offload cert SSL trên đầu Citrix, Nginx, Haproxy -> Proxy tới Ingress-Nginx Cluster Port 80/443

# # # *NOTE* 
Các services chạy trên cùng k8s cluster, chúng ta nên sử dụng service name để gọi giữa 2 services.

Trong trường hợp có nhiều cluster k8s:
  + Tạo ingress domain nội bộ để gọi local với nhau. Nên sử dụng bản ghi A và bản ghi CNAME.
  + Tạo bản ghi A: gw-test.k8s-vnpay.vnpay.local -> IP Ingress K8S Cluster
  + Tạo bản ghi CNAME:  xxx.k8s.vnpay.local -> gw-test.k8s-vnpay.vnpay.local

Trong trường hợp service đối tác gọi sang service VNPAY trên K8s: cần nghiên cứu thêm luồng này.