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

**Operating system list** 

> **Note:** **Client** คือ เครื่องที่จะเข้าถึงไฟล์ที่แชร์ด้วยการ mount ไปยัง **Server** คือ เครื่องที่จะแชร์ไฟล์

- [Ubuntu & Debian](#ubuntu-2204-lts-2004-lts-1804-lts-and-debian-11-10-9)
    - [Client](#Shere-FIle-Client)
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

To install Samba, we run | ติดตั้ง:

```bash
sudo apt update -y && sudo apt install samba -y
```

We can check if the installation was successful by running | เช็กว่า Samba ติดตั้งสำเร็จหรือไม่:
```bash
whereis samba
```

The following should be its output | ผลลัพธ์ จากการพิม > `whereis samba`:
```log
samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz
```

#### 2. Setting up Samba

Now that Samba is installed, we need to create a directory for it to share:
```bash
mkdir /home/<username>/sambashare/
```

The command above creates a new folder `sambashare` in our home directory which we will share later.

The configuration file for Samba is located at `/etc/samba/smb.conf`. To add the new directory as a share, we edit the file by running:

```bash
sudo nano /etc/samba/smb.conf
```

At the bottom of the file, add the following lines:
```smb.conf
[sambashare]
    comment = Samba on Ubuntu
    path = /home/username/sambashare
    read only = no
    browsable = yes
```

Then press `Ctrl-O` to save and `Ctrl-X` to exit from the nano text editor.

What we’ve just added
- comment: A brief description of the share.
- path: The directory of our share.
- read only: Permission to modify the contents of the share folder is only granted when the value of this directive is no.

- browsable: When set to yes, file managers such as Ubuntu’s default file manager will list this share under “Network” (it could also appear as browseable).

Now that we have our new share configured, save it and restart Samba for it to take effect:

```bash
sudo service smbd restart
```

Update the firewall rules to allow Samba traffic:
```bash
sudo ufw allow samba
```

#### 3. Setting up User Accounts and Connecting to Share

Since Samba doesn’t use the system account password, we need to set up a Samba password for our user account:

```bash
sudo smbpasswd -a username
```

Connecting to Share

On Windows, open up File Manager and edit the file path to:

```
\\ip-address\sambashare
```

> **Note:** ip-address is the Samba server IP address and sambashare is the name of the share.