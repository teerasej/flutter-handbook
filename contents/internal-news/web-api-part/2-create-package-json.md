
# สร้าง package.json เพื่อจัดการข้อมูลโปรเจคเป็นมาตรฐาน


1. เปิดโฟลเดอร์โปรเจค web-api ขึ้นมาใน Visual Studio Code
2. เปิด Terminal ขึ้นมาใช้งานใน Visual Studio Code
3. พิมม์คำสั่งด้านล่าง และกดปุ่ม Enter เพื่อรันคำสั่ง

```
npm init
```

4. ทำการกรอกข้อมูล และกด Enter ทีละขั้นตอน 
    - ถ้าไม่มีการกรอกข้อมูล ระบบจะใช้ค่าที่อยู่ในวงเล็บแทน 
    - ถ้าไม่มีค่าในวงเล็บจะเป็นค่าว่าง
    - สามารถ enter ผ่านจนครบแล้วมากรอกข้อมูลทีหลังได้ 

```bash
package name: (web-api) 
version: (1.0.0) 
description: 
entry point: (index.js) app.js <-- ส่วนนี้ให้ใส่ app.js เพราะโค้ดของเราอยู่ในไฟล์ชื่อดังกล่าว
test command: 
git repository: 
keywords: 
author: 
license: (ISC) 
Is this OK? (yes) y <--- พิพม์แค่ y และกด enter หรือจะ enter ผ่านไปเลยก็ได้ ถือว่ายืนยันเหมือนกัน
```

5. หากการยืนยันข้อมูลเสร็จสิ้น จะมีไฟล์ชื่อ **package.json** ปรากฎขึ้นมาในโฟลเดอร์โปรเจคของเรา

