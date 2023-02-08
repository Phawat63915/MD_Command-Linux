<!-- https://ubuntu.com/tutorials/install-and-configure-samba#4-setting-up-user-accounts-and-connecting-to-share -->

[Samba Server Guide](https://help.ubuntu.com/community/Samba/SambaServerGuide?_ga=2.22781115.1053081430.1675660950-291376188.1671546043)


**Check user and group**
```bash
id -u <username>
id -g <groupname>
```

**Check list user**
```bash
pdbedit -L -v
```

# Ubuntu

Ubuntu Server [Ubuntu release support timeline](https://ubuntu.com/about/release-cycle) คู่ใช้ได้กับ
- 22.04 LTS (Hirsute Hippo) 
- 20.04 LTS (Focal Fossa)
- 18.04 LTS (Bionic Beaver)
- 16.04 LTS (Xenial Xerus)

## Install and Configure Samba

### 1. Overview
เซิร์ฟเวอร์ไฟล์ Samba เปิดใช้งานการแชร์ไฟล์ระหว่างระบบปฏิบัติการต่างๆ ผ่านเครือข่าย ช่วยให้คุณเข้าถึงไฟล์เดสก์ท็อปจากแล็ปท็อปและแชร์ไฟล์กับผู้ใช้ Windows และ macOS

คู่มือนี้ครอบคลุมการติดตั้งและกำหนดค่า Samba บน Ubuntu

What you’ll learn
- Make Server File Shrer with Samba
- Share via local network & Network (LAN)

Requirements for doing
- Ubuntu 16.04 LTS or later
- Local Area Network (LAN) to share files over

### 2. Installing Samba

To install Samba, we run:

```bash
sudo apt update -y && sudo apt install samba -y
```

We can check if the installation was successful by running:
```bash
whereis samba
```

The following should be its output:
```log
samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz
```

### 3. Setting up Samba

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
----

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

### 4. Setting up User Accounts and Connecting to Share

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