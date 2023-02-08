<!-- https://ubuntu.com/tutorials/install-and-configure-samba#4-setting-up-user-accounts-and-connecting-to-share -->

[Samba Server Guide](https://help.ubuntu.com/community/Samba/SambaServerGuide?_ga=2.22781115.1053081430.1675660950-291376188.1671546043)


<!-- **Check user and group**
```bash
id -u <username>
id -g <groupname>
```

**Check list user**
```bash
pdbedit -L -v
``` -->
# Overview Install and Configure Samba
เซิร์ฟเวอร์ไฟล์ Samba เปิดใช้งานการแชร์ไฟล์ระหว่างระบบปฏิบัติการต่างๆ ผ่านเครือข่าย ช่วยให้คุณเข้าถึงไฟล์เดสก์ท็อปจากแล็ปท็อปและแชร์ไฟล์กับผู้ใช้ Windows และ macOS

คู่มือนี้ครอบคลุมการติดตั้งและกำหนดค่า Samba บน [👇](#operating-system-list)

<!-- What you’ll learn
- Make Server File Shrer with Samba Server & Client
- Share via local network & Network (LAN)

Requirements for doing
- Linux Operating System
- Local Area Network (LAN) to share files over -->

#### Operating system list

> **Note:** **Client** คือ เครื่องที่จะเข้าถึงไฟล์ที่แชร์ด้วยการ mount ไปยัง **Server** คือ เครื่องที่จะแชร์ไฟล์

- [Ubuntu & Debian](#ubuntu-2204-lts-2004-lts-1804-lts-and-debian-11-10-9)
    - [Client](#mount-shere-file-client)
    - [Server](#shere-file-server)





# Installation Manual
## Ubuntu 22.04 LTS, 20.04 LTS, 18.04 LTS and Debian 11, 10, 9
Ubuntu Server & Desktop
- 22.04 LTS (Hirsute Hippo) 
- 20.04 LTS (Focal Fossa)
- 18.04 LTS (Bionic Beaver)

Debian Server & Desktop
- 11 (Bullseye)
- 10 (Buster)
- 9 (Stretch)





### Shere FIle (Server)

#### 1. Installing Samba

ติดตั้ง Samba เราเรียกใช้คำสั่ง


```bash
sudo apt update -y && sudo apt install samba -y
```


ตรวจสอบว่า Samba การติดตั้งสำเร็จหรือไม่โดยเรียกใช้
```bash
whereis samba
```


_OUTPUT: ผลลัพธ์ที่ได้จะเป็น_ 👇 > `whereis samba`:
```log
samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz
```



#### 2. Setting up Samba

เมื่อติดตั้ง Samba แล้ว เราต้องสร้างไดเร็กทอรีสำหรับแชร์
```bash
mkdir /home/<username>/sambashare/
```
คำสั่งด้านบนสร้างโฟลเดอร์ใหม่ `sambashare` ในโฮมไดเร็กตอรี่ของเรา ซึ่งเราจะแบ่งปันในภายหลัง

ไฟล์การกำหนดค่าสำหรับ Samba อยู่ที่ `/etc/samba/smb.conf` หากต้องการเพิ่มไดเร็กทอรีใหม่เป็นการแชร์ เราแก้ไขไฟล์โดยเรียกใช้:


```bash
sudo nano /etc/samba/smb.conf
```
ที่ด้านล่างของไฟล์ ให้เพิ่มบรรทัดใหม่ ที่ `/etc/samba/smb.conf`
```smb.conf
[sambashare]
    comment = Samba on Ubuntu
    path = /home/username/sambashare
    read only = no
    browsable = yes
```

จากนั้นกด <kbd>Ctrl</kbd> <kbd>O</kbd> เพื่อบันทึกและ <kbd>Ctrl</kbd> <kbd>X</kbd> เพื่อออกจากโปรแกรมแก้ไขข้อความนาโน

What we’ve just added
- **comment**: คำอธิบายสั้น ๆ ของส่วนแบ่ง
- **path**: ไดเรกทอรีของการแบ่งปันของเรา
- **read only**: การอนุญาตให้แก้ไขเนื้อหาของโฟลเดอร์ที่ใช้ร่วมกันจะได้รับอนุญาตก็ต่อเมื่อค่าของคำสั่งนี้ไม่ใช่
- **browsable**: เมื่อตั้งค่าเป็นใช่ ตัวจัดการไฟล์เช่นตัวจัดการไฟล์เริ่มต้นของ Ubuntu จะแสดงรายการการแชร์นี้ภายใต้ "เครือข่าย" (อาจปรากฏเป็นเรียกดูได้)


ตอนนี้เราได้กำหนดค่าการแชร์ใหม่แล้ว ให้บันทึกและรีสตาร์ท Samba เพื่อให้มีผล
```bash
sudo service smbd restart
```


อัปเดตกฎไฟร์วอลล์เพื่ออนุญาตการรับส่งข้อมูลของ Samba:
```bash
sudo ufw allow samba
```



#### 3. Setting up User Accounts and Connecting to Share

> **Note:** ip-address is the Samba ของเซิร์ฟเวอร์ Samba และ sambashare คือชื่อของการแชร์

เนื่องจาก Samba ไม่ใช้รหัสผ่านบัญชีระบบ เราจำเป็นต้องตั้งรหัสผ่าน Samba สำหรับบัญชีผู้ใช้ของเรา
```bash
sudo smbpasswd -a username
```


Connecting to Share

ใน Windows ให้เปิด File Manager และแก้ไขพาธไฟล์เป็น
```
\\ip-address\sambashare
```




### Mount Shere FIle (Client)

เปิดไฟล์เพื่อแก้ไข
```bash
sudo nano /etc/fstab
```

เพิ่มบรรทัดนี้ลงไปในไฟล์ `//ip-address/sambashare /home/username/sambashare cifs username=<username>,password=<password>,uid=1000,gid=1000,iocharset=utf8 0 0`
```
//192.168.56.51/sambashare /root/sambashare cifs username=phawat,password=JKHWI@NI#@#_-#&,uid=1000,gid=1000,iocharset=utf8 0 0
```

reboot เพื่อให้มีผล
```bash
sudo reboot
```