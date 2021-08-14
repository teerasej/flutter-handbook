
# ทดสอบการทำงานของ Web API 


## ทดสอบด้วย Web Browser 

1. คลิกลิ้งค์ [http://localhost:3000](http://localhost:3000) เพื่อเปิด URL ขึ้นมาใน Web Browser
2. ควรเห็นคำว่า **Hello World** แสดงบนหน้าเว็บ 

- ในที่นี้โปรแกรม Web Browser ทำหน้าที่เป็น Client ที่ส่งคำขอ (Request) แบบ GET ไปที่ URL ของ Web API 

```js
// Web API รับ Request ตามที่ระบุตาม path 
app.get('/', function (req, res) {

  // และทำการรันคำสั่งตาม function ที่เราเขียน
  // Web API ตอบกลับด้วยข้อความ Hello World ผ่าน function .send()
  res.send('Hello World')
})
```

## ทดสอบด้วย PostMan 

1. เปิดโปรแกรม PostMan 
2. เลือก **New > HTTP Request**
3. กำหนดรายละเอียดของ Request ตามนี้ 

- **Method**: GET
- **URL**: http://localhost:3000/

4. กดปุ่ม **Send** 
5. จะเห็นว่ามีข้อความ **Hello World** ตอบกลับมาในส่วน Response
