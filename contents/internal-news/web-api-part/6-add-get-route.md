
# เพิ่ม route สำหรับส่งข้อมูลข่าวกลับไปให้ user

1. เปิดไฟล์ **app.js**
2. เขียนคำสั่งเรียกใช้ function `.get()` เพื่อสร้าง route path ใหม่

```js
// Route ที่ส่งข้อความกลับไปให้แอพ
app.get('/', function (req, res) {
  res.send('Hello World')
})

// สร้าง Route ใหม่ 
// คราวนี้กำหนด URL เป็น /news
app.get('/news', function(req, res) {
    
})
```

3. เราจะเขียนให้ function สำหรับ route **/news** คืนค่ากลับไปเป็นรูปแบบ JSON ที่ภาษาโปรแกรมทั่วไปสามารถอ่านค่าไปใช้งานได้ 

```js
app.get('/news', function(req, res) {

    // ใช้ function .json() คืนค่า Array ที่มีข้อมูล
    res.json([
        { content: 'abc' },
        { content: 'def' },
        { content: 'ghi' }
    ])
})
```

4. บันทึกไฟล์ ตรงนี้ตัว Web API ที่รันอยู่จำเป็นต้องมีการรีสตาร์ท
5. ทดสอบส่ง Request ไปที่ Web API 

```
http://localhost:3000/news
```

6. จะเห็นข้อมูล JSON แสดงขึ้นมา

## ลองเล่น

- ให้ทดสอบใช้ PostMan ส่ง request มาที่ Route `/news` ที่เพิ่งสร้างขึ้นมาใน Workshop นี้

- **Method**: GET
- **URL**: http://localhost:3000/news

## โค้ดไฟล์ app.json ที่ปรับปรุงแล้ว

```js
const express = require('express')
const app = express()
 
app.get('/', function (req, res) {
  res.send('Hello World')
})

app.get('/news', function(req, res) {
    res.json([
        { id: 0, content: 'abc' },
        { id: 1, content: 'def' },
        { id: 2, content: 'ghi' }
    ])
})

app.listen(3000, function(){
    console.log('server is running port 3000...')
})
```