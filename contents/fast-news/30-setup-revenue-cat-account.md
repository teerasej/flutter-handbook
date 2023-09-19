
# Setup RevenueCat Account (iOS)

## 1. สมัครสมาชิก RevenueCat

สมัครสมาชิก RevenueCat จากเว็บ https://www.revenuecat.com/

## 2. ทำการสร้างโปรเจคใหม่ 

จากแถบเมนูด้านบน เลือก Projects > Create new Project 

<img width="324" alt="2023-09-19_12-54-45" src="https://github.com/teerasej/flutter-handbook/assets/85179/4f47440c-1d66-4f0d-962c-1ef852b174aa">


และกรอกชื่อโปรเจคให้เรียบร้อย

<img width="420" alt="2023-09-18_23-33-32" src="https://github.com/teerasej/flutter-handbook/assets/85179/b7a12f3f-c2a3-4eb6-9fee-f4998a8eadf8">

## 3. เลือกเชื่อมต่อกับ App ของ App Store

<img width="920" alt="2023-09-18_23-33-46" src="https://github.com/teerasej/flutter-handbook/assets/85179/5851ab2f-c7d6-497a-a0e0-fa0d342387ad">

## 4. ทำตามขั้นตอนเพื่อเชื่อมต่อกับ App Store

สำหรับ iOS ให้[ใช้ข้อมูลจาก Link นี้](https://teerasej.notion.site/RevenueCat-The-Nation-e89eaebd3cee4555a17b07c9d4814327?pvs=4)

## 5. คัดลอก API Key ของแต่ละ Store

หลังจากเชื่อมต่อแล้ว เราจะสามารถนำ API Key ไปใช้ในแอพได้ 

<img width="1271" alt="2023-09-19_07-28-10" src="https://github.com/teerasej/flutter-handbook/assets/85179/80d2e68b-e48a-49ad-b45b-eae7c729ad57">

## 6. สร้าง Entitlement ของ Package 

<img width="1256" alt="2023-09-19_09-22-15" src="https://github.com/teerasej/flutter-handbook/assets/85179/83781619-f0f5-4839-8803-332e66ddc353">


## 7. สร้าง Product ที่เป็นตัวแทนของ Subscription จากฝั่ง App Store 

<img width="1272" alt="2023-09-19_09-22-32" src="https://github.com/teerasej/flutter-handbook/assets/85179/1a128280-cab8-417d-9d2e-e72694035cfe">

หากมีการตั้งค่า App Store Connect แล้ว เราจะสามารถ import product จาก App Store มาได้เลย

<img width="548" alt="2023-09-19_12-59-14" src="https://github.com/teerasej/flutter-handbook/assets/85179/4468e34e-283d-4664-88cd-6f037c51e5c2">


## 8. ทำการสร้าง Offering 

ส่วนนี้จะเป็นการสร้างและจัดการ Package ที่รวม Product ที่เลือกไว้ เพื่อให้ถูกดึงไปแสดงในแอพ และเลือกซื้อในแอพ

<img width="1277" alt="2023-09-19_13-01-35" src="https://github.com/teerasej/flutter-handbook/assets/85179/9508866b-c27a-4ca2-a4b0-8e24bd8f2953">


## 9. ทำการสร้าง Package เพื่อจัดชุด Product ใน Offering นี้ 

<img width="925" alt="2023-09-19_13-06-10" src="https://github.com/teerasej/flutter-handbook/assets/85179/c054845f-3525-4d42-90f8-dc7b563a706e">

จากนั้นเลือก Package เพื่อทำการเพิ่ม product

<img width="946" alt="2023-09-19_13-06-45" src="https://github.com/teerasej/flutter-handbook/assets/85179/36335ae2-cb58-49c9-b865-98f394ef4282">

ทำการเพิ่ม product ที่มีอยู่ในระบบ เข้าไปใน Package 

<img width="927" alt="2023-09-19_13-07-00" src="https://github.com/teerasej/flutter-handbook/assets/85179/c74e1a41-1900-4a74-91bb-5a0f6cf18797">

เลือก Product ที่มีการสร้างไว้

<img width="946" alt="2023-09-19_13-07-15" src="https://github.com/teerasej/flutter-handbook/assets/85179/35d05274-7d92-4768-abd2-9a5b76645a21">

## พร้อมใช้แล้วจ้า
