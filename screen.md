# การใช้คำสั่ง screen

1.สร้างหน้าจอ console ที่เวลาเราออก remote แล้วเซิฟเวอร์จะไม่ดับ -S เพื่อเปลี่ยนชื่อ แนะนำให้มี -S แล้วใส่ชื่อที่เราต้องการพิม "screen -S ชื่อหน้าจอ" เช่น
```
screen -S mc
```

2.คำสั่งเพื่อดู รายการทั้งหมด ของหน้าจอที่เราสร้าง
```
screen -ls
```

3.ในกรณีที่ออก remote ไปแล้ว หรือ เรากด ctrl + a + d ออกหรือ ออกหน้าจอที่เราสร้าง -r คือ เข้าปกติหากเราหลุดหรือมีคนเข้าอยู่จะไม่สามารถเข้าได้ แต่ -x จะทำงานตรงกันข้ามกันหากเราหยุดเข้าไม่ได้เราจะเข้าได้ หรือหากมีคนเข้าอยู่ เราจะเข้าได้
```
screen -r mc
screen -x mc
```