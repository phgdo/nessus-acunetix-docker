# nessus-acunetix-docker
nessus + acunetix docker


# Cracked Nessus in Docker
## Setup
````
sudo apt install docker.io
sudo usermod -aG docker $USER   #Log out and back in after this
docker pull ramisec/nessus
docker run -d --name=nessus -p 8834:8834 ramisec/nessus
````

## Update
Tạo một mail ảo trên https://temp-mail.org/en
Tạo một tài khoản Nessus Essentials trên https://tenable.com/products/nessus/nessus-essentials
Lấy activation code gửi về mail ảo
````
docker exec -it nessus /bin/bash
/opt/nessus/sbin/nessuscli fetch --challenge
````
Copy Challenge code: 41f9607d08d7f9fd43f00e5a13195489d1eda1e2
Truy cập https://plugins.nessus.org/v2/offline.php điền Challenge code và activation code 
Tải license về máy 
````
docker cp nessus.license nessus:/nessus/nessus.license

/opt/nessus/sbin/nessuscli fetch --register-offline /nessus/nessus.license
Kết quả ra như này là ok: Your Activation Code has been registered properly - thank you.
````

Cài đặt plugin
Trong trang https://plugins.nessus.org/v2/offline.php có thêm link để tải plugins update
````
docker cp all-2.0.tar.gz nessus:/tmp/all-2.0.tar.gz

/opt/nessus/sbin/nessuscli update /tmp/all-2.0.tar.gz
Kết quả ra như này là ok:
[info] Copying templates version 202506091647 to /opt/nessus/var/nessus/templates/tmp
[info] Finished copying templates.
[info] Moved new templates with version 202506091647 from plugins dir.
 * Update successful.  The changes will be automatically processed by Nessus.
````
