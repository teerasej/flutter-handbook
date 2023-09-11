
# สร้าง Route ของ Web API

1. เปิดไฟล์​ **app.js** ในโปรเจคของเรา
2. ลบโค้ดเดิมออก และเขียนโค้ดใหม่ตามด้านล่าง

```js
// เรียกใช้งาน package ชื่อ express
const express = require('express')
const app = express()

// กำหนด route GET / 
app.get('/', function (req, res) {

    // ใช้ response ส่งข้อความกลับไปให้แอพที่ส่งคำขอมาที่ route นี้
  res.send('Hello World')
})


// กำหนดให้ server เริ่มการทำงาน และรอรับ request ที่ส่งมาที่ port 3000
app.listen(3000, function(){

    // แสดงข้อความ เมื่อ server เริ่มการทำงานได้สมบูรณ์
    console.log('server is running at port 3000...')
})
```

3. บันทึกไฟล์ 

## การเปิดการทำงานของ Web API

1. เปิด Terminal ขึ้นมาใน Visual Studio Code
2. พิมพ์ และรันคำสั่งด้านล่าง 

```
node app
```

3. ควรจะเห็นว่ามีข้อความ **server is running at port 3000...** แสดงขึ้นมาใน Terminal
4. ให้สังเกตว่า Terminal จะหยุดรับคำสั่งเพิ่มเติม

## การปิดการทำงาน

1. หากต้องการปิดการทำงานของ Web API ให้คลิกเลือก Terminal ที่ Web API เปิดการทำงานไว้อยู่
2. กดปุ่มลัดบนคีย์บอร์ด **Ctrl + C** 
3. ถ้ามีการขึ้นข้อความถามเพื่อยืนยัน ให้พิมพ์​ **y** และกด enter
4. จะเห็นว่า Terminal กลับมารับคำสั่งได้อีกครั้ง จุดนี้ Web API ของเราสิ้นสุดการทำงานอย่างสมบูรณ์

> การปิดโปรแกรม Visual Studio Code หรือปิด Terminal ที่ Web API ทำงานอยู่ ถือเป็นการสิ้นสุดการทำงานเช่นเดียวกัน 

## เปิดการทำงานของ Node แบบที่รีสตาร์ทตัวเองได้ เวลามีการแก้ไขโค้ด

เราสามารถรันโค้ดของ JavaScript ที่สามารถรีสตาร์ทตัวเองได้ หากมีการแก้ไข และบันทึกการเปลี่ยนแปลงของโค้ด 

โดยใช้คำสั่งด้านล่าง รันไฟล์ app

```
nodemon app
```

## โค้ดไฟล์ app.json ที่ปรับปรุงแล้ว

```js
const express = require('express')
const app = express()

app.get('/', function (req, res) {
  res.send('Hello World')
})

app.listen(3000, function(){
    console.log('server is running at port 3000...')
})
```