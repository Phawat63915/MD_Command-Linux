# คำสั่งอื่นๆ


บล็อก ทางเข้า ip
```
iptables -A INPUT -s SERVERIP -j DROP
```

ยกเลิกกฏ บล็อกทางเข้า ip
```
iptables -D INPUT -s SERVERIP -j DROP
```


ดูรายการ ทางเข้าทั้งหมด
```
iptables -L INPUT -n --line-numbers
```

เครื่องมือเน็ต net-tools ดูรายการใช้ port ทั้งหมด
```
sudo netstat -tulpn
```

สลับเวอร์ชั้น java หากในเครื่องมีมากกว่า 1 version
```
sudo alternatives --config java
```

คำสั่งแตกไฟล์ สำหรับ winrar
```
unrar x filename.rar
```

คำสั่งบีบอัดไฟล์ขั้งสูง สำหรับ winrar
```
rar a -r -u -m5 serversave.rar "./SURVIVE-SS1-MAIN/*.*"
```