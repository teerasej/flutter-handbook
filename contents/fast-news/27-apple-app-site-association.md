
#  Setup iOS's Universal Link

สำหรับ iOS การทำงานของ Deep links จะมีรูปแบบของ Apple ที่เรียกว่า Universal Link จะเป็นการที่เราต้องมีการยืนยันความเป็นเจ้าของ domain name ที่จะผนวกเข้ากับตัวแอพพลิเคชั่น 

ซึ่งเราต้องอัพโหลดไฟล์ชื่อ **apple-app-site-association** ไปไว้ใน root directory ของ hosting ที่ผูกกับ Domain ดังกล่าว

โดยไฟล์ต้องสามารถเข้าถึงได้ตาม Pattern ต่อไปนี้ 

```
<domain name>/.well-known/apple-app-site-association
<domain name>/apple-app-site-association
```

> ⚠️ ไม่ต้องมีนามสกุลไฟล์ .json

## A. ตัวอย่างไฟล์ apple-app-site-association

```json
{
    "applinks": {
        "apps": [],
        "details": [
            {
                "appID": "th.in.nextflow.fastnewsapp",
                "paths": [ "*" ]
            }
        ]
    }
}
```

2 จุดสำคัญ ที่ต้องนำมาใส่ไว้ในไฟล์นี้ คือ 

1. **appID** สามารถเอามาได้จากไฟล์ Xcode > Runner  ในโปรเจค iOS ของ Flutter

<img width="459" alt="2023-09-17_18-16-22" src="https://github.com/teerasej/flutter-handbook/assets/85179/16836305-ad22-4c95-920f-1b1e8a7d6197">

2. เข้าไปที่ Target > Runner > Signing & Capabilities > Bundle Id

<img width="1117" alt="2023-09-18_22-11-26" src="https://github.com/teerasej/flutter-handbook/assets/85179/62d129b2-15d1-449a-93d1-c2f4f16e63f7">

3. ในที่นี้ไฟล์ **apple-app-site-association** จะไม่มีนามสกุล `.json` ทำให้ตัวระบบไม่สามารถอ่าน content-type ของไฟล์เป็น application/json ได้ 

อาจจะแก้ได้โดยใช้ไฟล์ `.htaccess` ใส่ไว้ในโฟลเดอร์เดียวกันกับไฟล์ **apple-app-site-association** ก็ได้ โดยเขียน rule กำหนดในไฟล์ `.htaccess` ดังนี้

```
<Files "apple-app-site-association">
ForceType 'application/json'
</Files>
```

## B. Setup Universal Link ใน Xcode

### 1. เปิด iOS โปรเจคใน Xcode 

<img width="459" alt="2023-09-17_18-16-22" src="https://github.com/teerasej/flutter-handbook/assets/85179/16836305-ad22-4c95-920f-1b1e8a7d6197">

### 2. เช็คดู bundle Id

<img width="1117" alt="2023-09-18_22-11-26" src="https://github.com/teerasej/flutter-handbook/assets/85179/62d129b2-15d1-449a-93d1-c2f4f16e63f7">

### 3. เพิ่ม Associated Domains capability

1. จาก Runner 
2. ในส่วนของ **Signing & Capability** กด **+ Capability**
3. ค้นหา **Associated Domains** และเลือก

<img width="763" alt="2023-09-17_18-18-48" src="https://github.com/teerasej/flutter-handbook/assets/85179/ab84006b-c2d1-429f-bb16-eb98f6e1ffc8">

### 4. เพิ่ม Domain

จากส่วน Associated Domains ให้เพิ่ม domain โดยกดปุ่ม **+** ตามรูปแบบด้านล่าง

```
applinks:<domain name>
```

เช่น 

```
applinks:nextflow.in.th
```

<img width="669" alt="2023-09-17_18-20-24" src="https://github.com/teerasej/flutter-handbook/assets/85179/dd3d40cb-5bfb-49a9-8475-8667388066b5">