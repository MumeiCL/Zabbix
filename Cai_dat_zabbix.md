# Cài đặt zabbix trên ubuntu 20.04
**Cài đặt zabbix server**
```
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.4+ubuntu20.04_all.deb
dpkg -i zabbix-release_latest_6.4+ubuntu20.04_all.deb
apt update
```
**Cài đặt Zabbix server, frontend, agent**
```
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```
**Cài đặt MariaDB**
```
sudo apt install software-properties-common -y
curl -LsS -O https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
sudo bash mariadb_repo_setup --mariadb-server-version=10.6
sudo apt update
sudo apt install -y mariadb-server-10.6 mariadb-client-10.6 mariadb-common
systemctl start mariadb
```
**Tạo cơ sở dữ liệu ban đầu**
```
mysql -uroot 
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> set global log_bin_trust_function_creators = 1;
mysql> quit;
```
**Import database**
```
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```
```
mysql -uroot 
mysql> set global log_bin_trust_function_creators = 0;
mysql> quit;
```
**Cấu hình DB cho Zabbix Server**
Sửa file /etc/zabbix/zabbix_server.conf
```
DBPassword=password
```
**Khởi chạy**
```
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```
mở trình duyệt web và truy cập http://IP-server/zabbix
## Addhost
**Cài đặt zabbix agent**
Trên máy ubuntu-server cần giám sát tiến hành cài đặt agent
```
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.4+ubuntu20.04_all.deb
dpkg -i zabbix-release_latest_6.4+ubuntu20.04_all.deb
apt update
apt install zabbix-agent
```
Sau đó và file ```/etc/zabbix/zabbix_agentd.conf``` và chỉnh sửa một số thông tin
```
Server=IP-zabbix-server
ServerActive=IP-zabbix-server
Hostname=ubuntu-server_192.168.88.100
```
**Khởi động lại agent**
```
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```
Tại trang quản trị zabbix tiến hành thêm host
![image](https://github.com/user-attachments/assets/798f411d-779b-4fcc-a21e-9aba977d9520)
Nội dung như sau
![image](https://github.com/user-attachments/assets/4da92180-fda9-4fce-a52a-80c5eb27276a)
Thêm thành công 
![image](https://github.com/user-attachments/assets/25edecc7-5434-468a-817d-dae76f8f9c18)


