

# เปิดที่อยู่ของ Web API ด้วย ngrok

จะทำส่วนนี้ได้ ต้อง[มีการติดตั้ง ngrok package ให้เรียบร้อย](../3-install-package.md) ก่อนนะ

ในระหว่างการพัฒนา Web API กับแอพพลิเคชั่น เช่นบนอุปกรณ์พกพา การทดสอบการทำงานร่วมกันนั้น แทนที่จะเสียเวลาในการติดตั้ง Web API และฐานข้อมูลบน Server จริง เราสามารถใช้ ngrok เชื่อมต่อได้ชั่วคราว

1. ให้แน่ใจว่าได้รัน Web API และ Database server ที่ทำงานร่วมกันไว้แล้ว 
2. เปิดอีก Terminal ขึ้นมาใน Visual Studio Code
3. รันคำสั่งด้านล่าง โดยระบุเลข port ลงท้าย

```bash
ngrok http 3000
```

โดยเลข port ควรตรงกับ port ของ Web API ที่เปิดทำงานอยู่ 

4. น่าจะเห็น Terminal แสดงข้อความประมาณด้านล่าง

```bash
ngrok by @inconshreveable
                                                                                                                          
Session Status                online       
Session Expires               1 hour, 59 minutes
Version                       2.3.40
Region                        United States (us) 
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://3fae1764fd5d.ngrok.io -> http://localhost:3000 
Forwarding                    https://3fae1764fd5d.ngrok.io -> http://localhost:3000
```

2 บรรทัดสุดท้าย คือ URL ที่เราสามารถเอาไปใช้อ้างอิงในแอพพลิเคชั่นได้ ซึ่งแต่ละครั้งจะไม่เหมือนกัน

## ทดสอบใน PostMan

### 1. ทดสอบ route GET /news 

ให้ทดสอบใช้ PostMan ส่ง request มาที่ Route `/news` โดยแทนที่ **ngrokURL** ด้วย URL ที่ได้จาก ngrok

- **Method**: GET
- **URL**: **ngrokURL**/news

ตัวอย่าง **http://3fae1764fd5d.ngrok.io/news**

### 2. ทดสอบ route POST /news/create 

ทดสอบใช้ PostMan ส่ง Request มาที่ Route `/news/create` โดยแทนที่ **ngrokURL** ด้วย URL ที่ได้จาก ngrok

- **Method**: POST
- **URL**: **ngrokURL**/news/create

ตัวอย่าง **http://3fae1764fd5d.ngrok.io/news/create**

โดยกำหมดส่งข้อมูลไปกับ request ด้วย

- ด้านล่างที่กรอก URL ในส่วน Params ให้เลือก **Body** tab
- ถัดลงมาให้เลือกการตั้งค่าเป็น **raw** และ **JSON** ตามลำดับ
- ในส่วนข้อความด้านล่างให้กรอก JSON ลงไปตามตัวอย่าง

```json
{
    "content": "123456"
}
```

1. กดปุ่ม **Send** 