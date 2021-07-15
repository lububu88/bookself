# OWNERS

Mỗi thư mục trong phần triển khai CD trên các môi trường cần có file OWNERS.

Trong file OWNERS này chứa thông tin của người chủ quản, người approver MR.

### Set up OWNERS
```
# OWNERS
approvers:
- team sre
- team pm, leader project
```

```
repo/
OWNERS
|__sub-dir-01/
|    |__OWNERS
|
|__sub-dir-02/
     |__OWNERS
```

```
# OWNERS SUBFOLDERs
approvers:
- dev 1
- dev 2
- team pm, leader project
```