# เพิ่ม route สำหรับสร้างข่าวใหม่จากข้อมูลที่ได้รับจากแอพมือถือ

1. เปิดไฟล์ **app.js**
2. เขียนคำสั่งเรียกใช้ function `.post()` เพื่อสร้าง route path ใหม่

```js
// ...

// Route /news ที่ส่งข้อมูลข่าวกลับไปที่แอพ
app.get('/news', function(req, res) {
    res.json([
        { id: 0, content: 'abc' },
        { id: 1, content: 'def' },
        { id: 2, content: 'ghi' }
    ])
})

// เพิ่ม Route สำหรับรับ POST Method จากแอพ
app.post('/news/create', function(req, res) {
    
    // กำหนดให้ web api ส่งสถานะ 200 ซึ่งหมายถึงการทำงานเสร็จสมบูรณ์กลับไปให้แอพ
    
    // เราสามารถใช้ function .json() เพื่อส่งข้อมูลกลับไปให้แอพเช่นกัน
    res.status(200).json({ message: 'ok' })

})
```

3. บันทึกไฟล์ และรีสตาร์ทการทำงานของ Web API ถ้าจำเป็น
4. ทดสอบใช้ PostMan ส่ง Request มาที่ Route `/news/create`

- **Method**: POST
- **URL**: http://localhost:3000/news/create

5. เราจะเห็นส่วน Response ของ PostMan แสดงรหัส **200 (OK)** เป็นส่วนที่ตอบกลับมาจาก Web API ตามที่เรากำหนด

### ลองเล่น

1. ทดลองเปลี่ยนโค้ดโปรแกรมให้ส่งรหัส 404 กลับมาแทน 200
2. บันทึกไฟล์ และรีสตาร์ท Web API
3. ทดสอบใช้ PostMan ส่ง Request มาที่ Route `/news/create` อีกครั้ง
4. **อย่าลืมเปลี่ยนรหัสกลับเป็น 200 ก่อนเริ่ม workshop ต่อไปล่ะ!**


## โค้ดไฟล์ app.json ที่ปรับปรุงแล้ว

```js
const express = require('express')
const app = express()
 
app.get('/', function (req, res) {
  res.send('Hello World')
})

app.get('/news', function(req, res) {
    response.json([
        { id: 0, content: 'abc' },
        { id: 1, content: 'def' },
        { id: 2, content: 'ghi' }
    ])
})

app.post('/news/create', function(req, res) {
    res.status(200)
})

app.listen(3000, function(){
    console.log('server is running port 3000...')
})
```