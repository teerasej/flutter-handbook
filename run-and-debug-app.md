
# Run & Debug Application

## วิธีสร้างโปรเจค Flutter ใน Visual Studio Code

- สำหรับ Android ให้แน่ใจว่าได้ทำการปลดล๊อค [Developer Option ตามขั้นตอนนี้แล้ว](https://nextflow.in.th/2014/enable-android-developer-option/)

1. เปิดโปรแกรม Visual Studio Code
2. เรียกใช้งาน Command Palette โดยไปที่เมนู **View > Command Palette** 

หรือใช้ปุ่มลัด
- Windows: Ctrl + Shift + P
- MacOS: Cmd + Shift + P

<img width="1023" alt="2021-08-14_22-32-37" src="https://user-images.githubusercontent.com/85179/129451369-d3d5bc6c-a356-4e09-a279-3ef6286eb903.png">


3. พิมพ์ค้นหา และเลือกคำส่ัง **Flutter: New Application Project**

<img width="691" alt="2021-08-14_22-22-53" src="https://user-images.githubusercontent.com/85179/129451343-9ae2fa99-b09f-4e60-b851-00fcb9aa7b7d.png">


5. ตั้งชื่อโปรเจคในกล่องข้อความให้เรียบร้อย

![2019-05-14_20-01-03 1bab8d8a15084f7fa241e330e039929e](https://user-images.githubusercontent.com/85179/66843057-6deea500-ef96-11e9-9f85-34def7a14a92.png)


5. เลือกโฟลเดอร์ที่จะสร้างโปรเจคเก็บไว้ โดยไปที่ directory ที่ต้องการเก็บโปรเจคในคอมพิวเตอร์ของเรา และคลิกปุ่ม **Select a folder to create project in** 

<img width="782" alt="2021-08-14_22-24-44" src="https://user-images.githubusercontent.com/85179/129451416-b09390d9-b795-4eed-ac89-9966ea348788.png">


7. รอจนระบบสร้างโปรเจคจนเสร็จ

## การเลือกอุปกรณ์เป้าหมายในการรันแอพพลิเคชั่น

1. เรียกใช้งาน Command Palette โดยไปที่เมนู **View > Command Palette** 

หรือใช้ปุ่มลัด
- Windows: Ctrl + Shift + P
- MacOS: Cmd + Shift + P

2. ค้นหา และเลือกคำสั่ง **Flutter: Select Device** 

<img width="745" alt="2021-08-14_22-37-05" src="https://user-images.githubusercontent.com/85179/129451538-83274bd3-def4-4f03-bef6-ef1a33771cf6.png">


3. จะปรากฎรายการของ Chrome, Android Emulator, iOS Simulator ที่พร้อมใช้บนเครื่องในขณะนั้น ให้คลิกเลือก Device ที่ต้องการ ก่อนจะเริ่มการทดสอบแอพพลิเคชั่น 



## วิธีรันทดสอบแอพพลิเคชั่นจาก Visual Studio Code

เราสามารถสั่ง Build และรันแอพพลิเคชั่นได้ทันที โดยกดปุ่ม **F5**

### กรณีที่กดปุ่ม F5 แล้วไม่ทำงาน

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
