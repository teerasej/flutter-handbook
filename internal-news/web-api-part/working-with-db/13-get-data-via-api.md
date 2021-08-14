
# เข้าถึง Collection จากใน Web API 

## 1. เรียกใช้ Collection ที่ต้องการใน Web API

```js
//...

// จะเห็นว่าการเชื่อมต่อฐานข้อมูล newsDB จะถูกเก็บไว้ในตัวแปร db ้ถ้าการเชื่อมต่อสมบูรณ์ เราจึงสามารถใช้ตัวแปร db ในการเข้าถึง collection ได้ 
const db = require('monk')('localhost/newsDB')

//...

// ในที่นี้ เราสามารถใช้ function .get() ในการเข้าถึง collection ได้ โดยการระบุชื่อลงไปตรงๆ 
let newsCollection = db.get('news')
```

## 2. ทำการเรียกใช้ข้อมูลจาก Collection และส่งกลับไปให้แอพพลิเคชั่น

เราจะมาปรับปรุงการทำงานของ route /news เพื่อให้ใช้ข้อมูลที่ได้จาก collection เป็นข้อมูลที่ตอบกลับไปให้แอพพลิเคชั่น

```js
app.get('/news', function(req, res) {

    // response.json([
    //     { content: 'abc' },
    //     { content: 'def' },
    //     { content: 'ghi' }
    // ])

    // function .find() เป็นการเรียกข้อมูลจาก collection ซึ่งสามารถกำหนด keyword หรือเงื่อนไขได้ ในที่นี้ไม่ได้กำหนดเงื่อนไขของข้อมูล เราจึงใส่เป็น `{}`
    // function .then จะรับข้อมูลที่ได้จากการทำงานของ function .find() เราตั้งชื่อข้อมูลนี้เป็นตัวแปรที่ชื่อว่า docs ซึ่ง docs จะมีข้อมูล หรือไม่มีก็ได้ ขึ้นอยู่กับตัว collection
    newsCollection.find({}).then(function(docs){

        // ใช้ res.json ในการแปลงข้อมูลของตัวแปร docs ให้เป็น json
        res.json(docs)
    })
})
```

## 3. ทดสอบการทำงาน

1. อย่าลืมว่าการเปลี่ยนแปลงโค้ดใน Web API จะมีผล ต่อเมื่อรีสตาร์ทการทำงานของ Web API แล้ว (หรือใช้ nodemon เพื่อลดขั้นตอนการทำงาน)
2. ทดสอบด้วย Web Browser ผ่าน url: http://localhost:3000/news
3. ทดสอบด้วย PostMan

- **Method**: GET
- **URL**: http://localhost:3000/news

**เราควรจะเห็นข้อมูลจากที่สร้างไว้ใน news collection ในรูปแบบ JSON แสดงขึ้นมาใน Web browser หรือ Postman ครับ**

## โค้ดไฟล์ app.json ที่ปรับปรุงแล้ว

```js
const express = require('express')
const db = require('monk')('localhost/newsDB')

const app = express()

app.use(express.json());

let newsCollection = db.get('news')
 
app.get('/', function (req, res) {
  res.send('Hello World')
})

app.get('/news', function(req, res) {
    newsCollection.find({}).then(function(docs){
        res.json(docs)
    })
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