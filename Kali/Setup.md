# Setup for Kali
### แก้ให้ใช้เน็ตได้
เข้าไปที /etc
```bash
cd /etc
```
แก้ไขไฟล์ `resolv.conf`  และแก้ nameserver 8.8.8.8
```bash
sudo vi resolv.conf
```
ทดสอบลองเครือข่าย
```bash
ping google.com
```
### อัพเดท
```bash
sudo apt update
```
```bash
sudo apt upgrade
```
```bash
sudo apt-get update
```
### เพิ่ม ภาษาไทยให่สามารถอ่านไทยได้
```bash
sudo install fonts-thai-tiwg
```
### Install SSH
```bash
sudo apt-get install ssh
```
```bash
systemctl restart ssh.service
```
```bash
systemctl enable ssh.service
```
```bash
systemctl status ssh.service
```