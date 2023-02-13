<!-- https://ubuntu.com/server/docs/service-nfs -->

# NFS Protocol (For Linux)
<!-- [youtube how to](https://www.youtube.com/watch?v=YcnqY86qwKM) -->

Lab: NFS Sharing
- Linux NFS Server centOS7 คนแชร์ (192.168.56.101)
- Linux NFS Client centOS7 ลูกแชร์ (192.168.56.102)
- Windows client (Windows 10) ลูกแชร์ (192.168.56.1)
----------------------------------------------------
ติดตั้ง nfs service บน linux (ทั้ง Server และ Client) :
```sh
yum install -y nfs-utils nfs-utils-lib
```
## Server Side
-----------
#### 1.Check service nfs :
```sh
service nfs-server status
```
#### 2.Allow firewalld port below :
```sh
firewall-cmd --permanent --add-port=111/tcp
firewall-cmd --permanent --add-port=54302/tcp
firewall-cmd --permanent --add-port=20048/tcp
firewall-cmd --permanent --add-port=2049/tcp
firewall-cmd --permanent --add-port=46666/tcp
firewall-cmd --permanent --add-port=42955/tcp
firewall-cmd --permanent --add-port=875/tcp

firewall-cmd --reload
```
#### 3.สร้าง Shared Directory :
```sh
mkdir /var/shared
chmod 777 /var/shared
```

#### 4.Create Export list on NFS (กำหนดลูกแชร์) :
```sh
vi /etc/exports
/var/shared	192.168.56.0/24(rw,sync,no_root_squash)
```
#### 5.Restart NFS service :
```sh
service nfs-server restart
```
#### 6.List shared export on server :
```sh
showmount -e 192.168.56.101
```
## Client Side
-----------
#### 1.สร้าง mount point สำหรับ mount shared และทำการ mount NFS
```sh
mkdir /var/client-shared
mount -t nfs 192.168.56.101:/var/shared/ /var/client-shared/
```
#### 2.Verify mount 
```sh
df -h
mount
```

## Mount NFS to Windows
--------------------
1.Start > Turn windows features on or off
2.Select service for NFS
3.CMD
```
mount 192.168.56.101:/var/shared S:\
```

## Fix mount folder missing after restart
จากการใช้คำสั่งด้านบน หาก Restart Server mount จะถูกยกเลิกและหายไป วิธีการแก้ทำตามขั้นตอนนี้
#### 1.ให้เปิดแก้ไขไฟล์
```bash
sudo nano /etc/fstab 
```
#### 2.เพิ่มบรรทัดด้านล่าง ไปที่ `/etc/fstab`
`Server:/path/to/export /local_mountpoint nfs <options> 0 0`
```fstab
192.168.0.216:/mnt/HDD1 /media/freenas/ nfs rw,bg,soft,intr,nosuid 0 0
```