# ติดตั้ง package สำหรับช่วยสร้าง Web API และเชื่อมต่อฐานข้อมูล

1. เปิดโฟลเดอร์โปรเจค **web-api** ขึ้นมาใน Visual Studio Code
2. เปิด Terminal ขึ้นมาใช้งานใน Visual Studio Code
3. พิมพ์คำสั่งด้านล่าง และกด enter ทีละคำสั่ง

```
npm i express
npm i monk
```

> [Express](https://www.npmjs.com/package/express) และ [Monk](https://www.npmjs.com/package/monk) เป็นชุดคำสั่งโปรแกรมที่มีคนสร้างไว้ เพื่ออำนวยความสะดวกในการทำงาน อยู่ในรูปแบบ Open Source 

4. ระบบจะทำการดาวน์โหลดและติดตั้ง package มาไว้ในโปรเจค ที่อยู่ของแพคเกจจะอยู่ใน directory ชื่อ **node_modules**
5. ลองเปิดไฟล์ **package.json** จะเห็นว่ามีชื่อของ package ที่เราสั่งติดตั้งอยู่ในส่วนที่เรียกว่า `dependencies`

```json
"dependencies": {
    "express": "^4.17.1",
    "monk": "^7.3.4"
}
```

## ติดตั้ง package ที่เป็นเครื่องมืออำนวยความสะดวกในการทำงาน

1. เปิด Terminal ขึ้นมาใช้งานใน Visual Studio Code
2. พิมพ์คำสั่งด้านล่าง และกด enter ทีละคำสั่ง

```bash
npm install -g nodemon
npm install -g ngrok
```

## ข้อดีของการที่มีการระบุชื่อ package ที่ติดตั้งในไฟล์ package.json
- เวลาย้ายโปรเจคไปไว้บนคอมเครื่องใหม่ หรือ server สำหรับ deploy ตัว Web API เราไม่ต้องก้อปปี้ directory ที่ชื่อ **node_modules** ไปด้วย
- นั่นคือเอาเฉพาะไฟล์อื่นๆ รวมถึง **package.json** ไปวางไว้บนเครื่องที่้ต้องการ (ที่ติดตั้ง node runtime ไว้เรียบร้อย) แล้วรันคำสั่ง `npm i` เพื่อติดตั้ง package อีกครั้ง