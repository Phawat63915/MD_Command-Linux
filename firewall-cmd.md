# คำสั่ง firewall-cmd

คำสั่งเปิด port firewall
```
firewall-cmd  --permanent --add-port=25565/tcp
```

ปิด หรือ ลบ port ที่เราเปิด ใน firewall
```
firewall-cmd  --permanent --remove-port=25565/tcp
```

reload หรือ อัพเดท ตั้งค่า firewall
```
firewall-cmd --complete-reload
```

ดูรายการ กฏ & port ที่เปิดทั้งหมด
```
firewall-cmd  --list-all
```
