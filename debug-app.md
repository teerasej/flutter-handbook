

# วิธีรันทดสอบแอพพลิเคชั่นจาก Visual Studio Code

ให้แน่ใจว่า ได้[ทำการเลือกอุปกรณ์ที่จะรันทดสอบแอพพลิเคชั่น](select-target-device.md)ที่ต้องการเรียบร้อยแล้ว

เราสามารถสั่ง Build และรันแอพพลิเคชั่นได้ทันที 

- โดยการกดปุ่ม **F5**
- หรือคลิกขวาที่ไฟล์ `lib/main.dart` แล้วเลือกคำสั่ง **start debugging**

## กรณีที่กดปุ่ม F5 แล้วไม่ทำงาน หรือ Debug ไม่ทำงาน

สามารถเริ่มต้นโดยเข้าจากโหมด Debug ก็ได้ 

1. เลือกโหมด **Debug**

![2019-05-14_20-03-54 bdf623f488a8477ebf849ef2ffa18fe5](https://user-images.githubusercontent.com/85179/66843090-7b0b9400-ef96-11e9-8bd2-c1a56d9fe286.png)


2. จากเมนู Dropdown ที่แสดงว่า **No Configuration** ให้เลือกคำสั่ง **Add Configuration**

![2019-05-14_20-05-12 8ead1b990b134616b5073da270f5fdab](https://user-images.githubusercontent.com/85179/66843117-88c11980-ef96-11e9-8805-813852722dc7.png)


3. หากเป็นครั้งแรกจะมีการดาวน์โหลดระบบเพิ่มเติมมาจาก Internet เสร็จแล้วจะมีการสร้างไฟล์ `launch.json` ซึ่งจะเป็นการตั้งค่าสำหรับโปรเจค Flutter

![2019-05-14_20-05-27 8ae1a0827f474957b95ee52b5ded122a](https://user-images.githubusercontent.com/85179/66843154-94acdb80-ef96-11e9-956c-8937e517b998.png)

4. เปิด Command Palette และใช้คำสั่ง **Flutter: Select Device** เพื่อเลือกชื่ออุปกรณ์ที่เชื่อมต่อกับคอมพิวเตอร์ หรือ Simulator ที่ต้องการ

![2019-05-14_20-08-08 027677149c114fd4acceb6b61d2226b1](https://user-images.githubusercontent.com/85179/66843184-a0000700-ef96-11e9-9ec3-f3d99faf61c3.png)

5. กดปุ่ม **Start Debug**

## สำหรับพวกเราที่ต้องการทดสอบบนอุปกรณ์ iOS ด้วย Apple ID ที่ยังไม่เสียเงิน

1. Apple ID ที่ยังไม่เสียเงินลงทะเบียน Apple Developer Account จะมีข้อจำกัดมากกว่าแบบที่เสียเงินแล้ว
2. การรันแอพบนอุปกรณ์ iOS ครั้งแรก แอพจะถูกติดตั้งบนอุปกรณ์ แต่จะไม่สามารถรันแอพขึ้นมาได้ [มีขั้นตอนที่ต้องทำเพิ่มเติมดังนี้](run-ios-app-with-free-apple-account.md)