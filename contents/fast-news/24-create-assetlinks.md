
# สร้างไฟล์ assetlinks.json

สำหรับ Android การทำงานของ Deep links จะเป็นการที่เราต้องมีการยืนยันความเป็นเจ้าของ domain name ที่จะผนวกเข้ากับตัวแอพพลิเคชั่น 

ซึ่งเราต้องอัพโหลดไฟล์ชื่อ **assetlinks.json** ไปไว้ใน root directory ของ hosting ที่ผูกกับ Domain ดังกล่าว


## A. ตัวอย่างไฟล์ assetlinks.json

```json
[
  {
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
      "namespace": "android_app",
      "package_name": "th.in.nextflow.fastnewsapp",
      "sha256_cert_fingerprints":
        ["12:34:..."]
    }
  }
]
```

2 จุดสำคัญ ที่ต้องนำมาใส่ไว้ในไฟล์นี้ คือ 

1. **package_name** สามารถเอามาได้จากไฟล์ android/app/build.gradle ในโปรเจค Flutter

```gradle
<!-- android/app/build.gradle -->
android {

    ....

    defaultConfig {
       
        applicationId "th.in.nextflow.fastnewsapp"
        ...
    }
```

2. **sha256_cert_fingerprints** ซึ่งสามารถใช้ตัวที่ลิ้งค์กับ App Bundle release ใน [Google Play Console](https://play.google.com/console/) ได้


## B. วิธีการสร้าง SHA-256 certificate fingerprint สำหรับแอพบน Google Play Console

1. Login เข้า Google Play Console
2. ถ้ายังไม่มี Profile ของ App ให้คลิก **Create App** ก่อน (แต่ถ้ามีแอพแล้วก็เลือก และไปต่อข้อ 4 ได้เลย)

<img width="955" alt="2023-09-16_21-14-25" src="https://github.com/teerasej/flutter-handbook/assets/85179/a5e9fc6f-825d-4aa9-9bb9-46ab7b03db37">

3. ทำการกรอกข้อมูล เช่นชื่อแอพ ภาษาเริ่มต้นของตัวแอพ, ประเภทของแอพ, การคิดค่าบริการของแอพ, และการยอมรับเงื่อนไขการใช้งาน ก่อนกดปุ่ม create

<img width="798" alt="2023-09-16_21-15-05" src="https://github.com/teerasej/flutter-handbook/assets/85179/0e601f7e-07e1-4743-9db1-ae3472189aa3">

4. **จากเมนูด้านซ้าย** เลือก Release > App integrity > Play app signing ซึ่งถ้ายังไม่มีส่วนของ App Signing ก็จะต้องสร้างขึ้นมาก่อน (ถ้ามีอยู่แล้ว ก็กด setting ทางด้านขวาได้เลย)

<img width="774" alt="2023-09-17_11-11-45" src="https://github.com/teerasej/flutter-handbook/assets/85179/df96fb1e-597c-4b22-a482-3047ab55b19d">

5. การใช้งานกลไก App Signing จะทำควบคู่กับการสร้าง Release (โปรไฟล์ที่ใช้ในการ Release แอพแต่ละเวอร์ชั่น) ถ้าไม่มี ตรงนี้แหละที่ต้องสร้างขึ้นมา ทำได้โดยการคลิกปุ่ม **Create release**

<img width="1094" alt="2023-09-17_11-13-27" src="https://github.com/teerasej/flutter-handbook/assets/85179/d8579e71-b93a-4df9-90a8-6138448ec43f">

6. ในหน้า Production เราจะสามารถกดสร้าง release ได้ 2 จุด จุดไหนก็ได้

<img width="718" alt="2023-09-17_11-14-06" src="https://github.com/teerasej/flutter-handbook/assets/85179/b9564e15-21c1-459e-9d19-3048a4c1b2bf">

7. ในส่วน Create production release นี้แหละ ที่เราสามารถเลือก App integrity > Choose signing key ได้ 

<img width="640" alt="2023-09-17_11-14-28" src="https://github.com/teerasej/flutter-handbook/assets/85179/9b2db063-c96e-42b3-b439-4020459a95dc">

8. เลือก **Use Google generated key** เพื่อให้ Google เก็บ key เพื่อป้องกันการสูญหาย 

พอกดยืนยันแล้ว รอสักพัก ระบบถึงจะสร้าง Key ให้เสร็จสมบูรณ์

<img width="725" alt="2023-09-17_11-14-33" src="https://github.com/teerasej/flutter-handbook/assets/85179/a15320cb-ccdd-4840-a95c-ebc307287984">

9. หลังจากการสร้าง key เสร็จสิ้น เราสามารถกลับมาในส่วน Release > App integrity ให้สังเกตจุด (1) ว่า App signing ของเราสถานะเป็น signing by Google Play

(2) ลงมาในส่วน Play app signing และคลิก setting ทางด้านขวา

<img width="1068" alt="2023-09-17_11-15-32" src="https://github.com/teerasej/flutter-handbook/assets/85179/e7a4d867-0393-483c-8be9-085d2e5df420">

10. จะเปิดมาหน้า App signing ซึ่งจะมี Key ต่างๆ อยู่ ส่วนที่เราต้องการเอาไปใช้เป็นไฟล์ assetlinks.json จะอยู่ในหัวข้อ Digital Asset Links JSON ซึ่งเราสามารถกดปุ่ม copy เอามาใส่ไว้ในไฟล์ได้ 