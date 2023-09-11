
# เพิ่มข้อมูลลง Collection จาก Web API 

## 1. นำข้อมูลที่ได้จากแอพพลิเคชั่น บันทึกลง Collection 

เราจะมาปรับปรุงการทำงานของ route `/news/create` เพื่อให้บันทึกข้อมูลที่ได้จากแอพพลิเคชั่น ลงไปใน Collection ของฐานข้อมูล

```js
app.post('/news/create', function(request, response) {

    let news = request.body

    // console.log(news)
    
    // เรียกใช้ Function .insert() ของ collection ทำให้เราสามารถส่ง object javascript ลงไปบันทึกใน collection ที่เรียกใช้ได้
    newsCollection.insert(news)

    response.status(200).json({ message: 'ok' })

})
```

## 2. ทดสอบ 

ทดสอบใช้ PostMan ส่ง Request มาที่ Route `/news/create`

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
7. ถ้าเราได้ status 200 กลับมาใน Postman ให้ลองเปิด Mongo Compass เพื่อตรวจสอบว่าข้อมูลเข้าไปอยู่ใน collection แล้วหรือไม่

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

    newsCollection.insert(news)

    res.status(200).json({ message: 'ok' })

})

app.listen(3000, function(){
    console.log('server is running port 3000...')
})
```