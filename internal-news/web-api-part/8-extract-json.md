
# ทำให้ Web API สามารถอ่านข้อมูล JSON ที่ได้รับมาจากแอพ

1. เปิดไฟล์ **app.js**
2. เพิ่มโค้ดโปรแกรมตามตัวอย่างด้านล่าง 

```js
const express = require('express')
const app = express()

// ถัดจากการสร้างตัวแทนของแอพ จะเป็นที่ๆ เหมาะสำหรับการตั้งค่าให้ app ที่เป็น web api ของเรา

// ตั้งค่าการทำงานของ Web api ให้อ่านข้อมูล JSON ได้
app.use(express.json());

//...
```

3. เพิ่มโค้ดโปรแกรมเข้าไปใน route `\news\create`

```js
app.post('/news/create', function(req, res) {

    // ข้อมูล json ที่ส่งมาจากแอพมือถือ จะถูกเก็บอยู่ในค่าที่ชื่อว่า .body ของ req
    // ในที่นี้เราเอาค่าดังกล่าวมาเก็บไว้ในตัวแปร news
    let news = req.body

    // เราใช้คำสั่ง console.log แสดงสิ่งที่อยู่ในตัวแปร news เพื่อให้แสดงข้อความขึ้นมาใน console 
    console.log(news)

    res.status(200).json({ message: 'ok' })

})
```

4. บันทึกไฟล์ และรีสตาร์ทการทำงานของ Web API ถ้าจำเป็น
5. ทดสอบใช้ PostMan ส่ง Request มาที่ Route `/news/create`

- **Method**: POST
- **URL**: http://localhost:3000/news/create

คราวนี้เราจะมีการส่งข้อมูลไปกับ request ด้วย

- ด้านล่างที่กรอก URL ในส่วน Params ให้เลือก **Body** tab
- ถัดลงมาให้เลือกการตั้งค่าเป็น **raw** และ **JSON** ตามลำดับ
- ในส่วนข้อความด้านล่างให้กรอก JSON ลงไปตามตัวอย่าง

```json
{
    "content": "123456"
}
```

6. กดปุ่ม **Send** 
7. สังเกตการทำงานของ server ใน Terminal ถ้ามีการแสดงข้อความแบบเดียวกับ JSON ที่เรากรอกไว้ใน PostMan แปลว่าทางฝั่ง Web API ได้ข้อมูลดังกล่าวแล้ว

## โค้ดไฟล์ app.json ที่ปรับปรุงแล้ว

```js
const express = require('express')
const app = express()

app.use(express.json());
 
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

app.post('/news/create', function(req, res) {

    let news = req.body

    console.log(news)

    res.status(200).json({ message: 'ok' })

})

app.listen(3000, function(){
    console.log('server is running port 3000...')
})
```