
# ใส่เงื่อนไข การ sort ข้อมูลตอนเรียกจาก MongoDB

## 1. กำหนดเงื่อนไขการเรียงข้อมูลที่ได้จาก collection

```js
app.get('/news', function(req, res) {

    // function .find() รับ parameter เป็นค่าที่ 2 ในการ sort ข้อมูลที่ได้จาก collection ได้ 
    // ในที่นี้เรากำหนดให้เรียง โดยใช้ _id ล่าสุดขึ้นเป็นอันแรก
    newsCollection.find({}, {sort: {_id: -1}}).then(function(docs){
        res.json(docs)
    })
})
```

## 2. ทดสอบการทำงาน

1. อย่าลืมว่าการเปลี่ยนแปลงโค้ดใน Web API จะมีผล ต่อเมื่อรีสตาร์ทการทำงานของ Web API แล้ว (หรือใช้ nodemon เพื่อลดขั้นตอนการทำงาน)
2. สามารถทดสอบโดย[เพิ่ม document เข้าไปใน collection ด้วยโปรแกรม Mongo Compass](11-init-data.md) อีก 2-3 document ได้
3. ทดสอบด้วย Web Browser ผ่าน url: http://localhost:3000/news
4. ทดสอบด้วย PostMan

- **Method**: GET
- **URL**: http://localhost:3000/news

**เราควรจะเห็นข้อมูลที่เรียงตามลำดับการสร้าง ผ่าน Web browser หรือ Postman ครับ**

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
    newsCollection.find({},{sort: {_id: -1}}).then(function(docs){
        res.json(docs)
    })
})

app.post('/news/create', function(req, res) {

    let news = req.body

    // save 
    console.log(news)

    res.status(200).json({ message: 'ok' })

})

app.listen(3000, function(){
    console.log('server is running port 3000...')
})
```