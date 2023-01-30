# คำสั่งเริ่ม แรกที่ต้องทำเมื่อเราได้เครื่อง ใน Linus Centos 8 & 7

ตรวจสอบให้แน่ใจว่าเปิดใช้งาน repo EPEL และติดตั้งบน CentOS 8 ถ้าไม่ให้รัน คำสั่ง
```
sudo dnf -y install epel-release
sudo dnf repolist
```

 ค้นหาแพ็คเกจที่เราจะติดตั้งหรือใช้งาน ดังนั้นพิมพ์คำสั่งต่อไปนี้เพื่อค้นหาแพ็คเกจ htop โดยใช้คำสั่ง
```
sudo dnf search htop
```

ค้นหาข้อมูลเกี่ยวกับแพ็คเกจ htop
```
sudo dnf info htop
```

อัพเดท แพ็คเกจ คำสั่ง yum
```
sudo dnf update -y
```

อัพเดทคำสั่ง yum
```
sudo dnf upgrade
```

ทำการติดตั้ง แพ็คเกจ htop เพื่อใช้ในการดูโปรแกมที่ใช้งานอยู่
```
sudo dnf install htop -y
```

ติดตั้ง แพ็คเกจ java 11 ลงในเครื่องเพื่อให้สามารถใช้งานได้
```
sudo dnf install java-16-openjdk -y
```

ติดตั้ง แพ็คเกจ screen เพื่อให้เซิฟมายคราฟไม่ปิดเมื่อเรา ออก remote
```
sudo dnf install screen -y
```