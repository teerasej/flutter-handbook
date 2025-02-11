
# การสร้างไฟล์ IPA (iOS) ด้วย Xcode

ในที่นี้จะเป็นขั้นตอนการสร้างไฟล์ IPA สำหรับ iOS โดยใช้ Xcode โดยเราจะใช้ Xcode บน macOS ในการสร้างไฟล์ IPA สำหรับ iOS

## 1. การเปิดโปรเจค Flutter ใน Xcode

1. เปิดโปรเจค Flutter ที่เราสร้างไว้ใน Visual Studio Code โดยการคลิกขวาที่โฟลเดอร์ของโปรเจค แล้วเลือก `Open in Xcode`
2. รอ Android Studio โหลดโปรเจคเสร็จ


## 2. การเช็ค Developer Account

1. จาก **Project Navigator** ทางด้านซ้าย คลิกเลือก **Runner** 
2. จาก Runner Configuration คลิกที่ **Signing & Capabilities** จากเมนู tab ที่อยู่ตรงกลางด้านบน
3. ให้แน่ใจว่ามีการตั้งค่าให้ถูกต้อง โดยเช็คในส่วน **Signing** ว่ามีการเลือก **Automatically manage signing** 
4. ในส่วน Bundle Identifier ให้ตรวจสอบว่ามีการตั้งค่าให้ถูกต้องหรือไม่ เช่น `com.example.myapp` หรือ `com.mycompany.myapp` หรือตามที่เราต้องการ
5. เช็คในส่วนของ **Team** ว่ามี Developer Account อยู่หรือไม่ ถ้าไม่มีให้เลือก **Add an Account** แล้วเพิ่ม Developer Account ของ Apple ของเราเข้าไป
6. รอให้ Xcode ทำการตรวจสอบ Developer Account ของเรา และทำการ setup ไฟล์ provision ให้เรียบร้อย 

> หากมีข้อผิดพลาด จะมีการแจ้งเตือน ให้ทำการแก้ไขตามคำแนะนำ เมื่อเสร็จแล้วให้กดปุ่ม **Fix Issue** ที่อยู่ด้านล่างขวามือ
   
## 3. การสร้างไฟล์ IPA

1. จากเมนู **Product** ด้านบน ให้เลือก **Archive**
2. รอให้ Xcode ทำการ Build โปรเจคของเรา และทำการ Archive โปรเจคของเรา
3. เมื่อเสร็จแล้ว จะมีหน้าต่างใหม่ขึ้นมา ให้เลือก **Validate App** เพื่อทำการตรวจสอบไฟล์ IPA ของเรา กับ App Store Connect
4. หากไม่มีข้อผิดพลาด ให้เลือก **Distribute App** เพื่อทำการอัพโหลดไฟล์ IPA ของเรา ไปยัง App Store Connect