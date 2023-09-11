
# เชื่อมต่อฐานข้อมูล MongoDB จาก Node Web API


1. เปิดไฟล์ **app.js**
2. เพิ่มโค้ดโปรแกรมตามตัวอย่างด้านล่าง 

```js
const express = require('express')

// เรียกใช้ monk และกำหนดค่าให้เชื่อมต่อไปที่ URL ของ MongoDB Server ที่เปิดทำงานอยู่ โดยปกติสามารถใช้ชื่อ localhost อ้างอิงได้
// เราสามารถระบุชื่อของฐานข้อมูลได้ ตรงนี้เราตั้งชื่อว่า newsDB จากจุดนี้ถ้าไม่มีฐานข้อมูลดังกล่าว ก็จะมีการสร้างขึ้นให้อัตโนมัติ
const db = require('monk')('localhost/newsDB')

```

## โค้ดไฟล์ app.json ที่ปรับปรุงแล้ว

```js
const express = require('express')
const db = require('monk')('localhost/newsDB')

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

    // save 
    console.log(news)

    res.status(200).json({ message: 'ok' })

})

app.listen(3000, function(){
    console.log('server is running port 3000...')
})
```