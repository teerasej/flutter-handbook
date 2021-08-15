
# เปิดการใช้งาน Cors

- โดยปกติ Web Server จะมีการป้องกันการส่ง Request มาจาก JavaScript เรียกว่า CORS 
- เว็บไซต์ในปัจจุบัน มีการทำงานในลักษณะนี้ค่อนข้างมาก 
- Web Application ที่สร้างจาก Flutter ก็รวมอยู่ในกรณีนี้เช่นกัน
- การส่ง Request จาก PostMan และ Web Browser โดยตรง**จะไม่รวมอยู่ในกรณีนี้**

## ไม่ได้เปิด CORS บน Web API จะเกิดอะไรขึ้น

ซึ่งถ้า Web API ของเราไม่ได้เปิดการทำงาน CORS ไว้ จะทำให้ Web API ปฏิเสธ Request และส่ง Error กลับมา

```bash
XMLHttpRequest error.
```

## วิธีการเปิด CORS บน Express 

1. ต้องมีการ[ติดตั้ง package CORS ในโปรเจค Node ให้เรียบร้อย](../3-install-package.md) ก่อน
2. ใช้ function `.use()` ของ app ในการเปิดใช้งาน CORS

```js
// เรียกใช้งาน cors package
const cors = require('cors')

const express = require('express')
const db = require('monk')('localhost/newsDB')

const app = express()

app.use(express.json())

// เพิ่มการใช้งาน CORS ลงไปใน express
app.use(cors())
```

## โค้ดไฟล์ app.json ที่ปรับปรุงแล้ว

```js
const cors = require('cors')
const express = require('express')
const db = require('monk')('localhost/newsDB')

const app = express()

app.use(express.json())
app.use(cors())

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

    newsCollection.insert(news)

    res.status(200).json({ message: 'ok' })

})

app.listen(3000, function(){
    console.log('server is running port 3000...')
})
```
