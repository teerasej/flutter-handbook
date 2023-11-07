
# Setup 

## 1. ติดตั้ง Flutter SDK 

- [วิธีติดตั้งสำหรับ MacOS](https://nextflow.in.th/2019/google-flutter-prepare-macos-for-ios-android-dev/)
- [วิธีติดตั้งสำหรับ Windows](https://nextflow.in.th/2018/setup-google-flutter-windows/)

## 2. ติดตั้ง Visual Studio Code Extension สำหรับการพัฒนาแอพด้วย Flutter

กดติดตั้งชุดรวมส่วนเสริม ได้จากที่นี่: [Nextflow’s Flutter Extension Pack](https://marketplace.visualstudio.com/items?itemName=teerasej.nextflow-s-flutter-extension-pack) (เข้าไปแล้วกดปุ่ม install)

## 3. สร้าง Emulator ของ Android (อีกชื่อคือ Android Virtual Device)

หากเป็น Windows จะไม่สามารถสร้าง Android Emulator ได้ ถ้าเข้าเงื่อนไขต่อไปนี้ 

- Hardware ของเครื่องไม่รองรับ Virtualization หรือเปิดอยู่
  
[ดูวิธีสร้าง Emulator (Android Virtual Device)](https://nextflow.in.th/2019/android-how-to-create-virtual-device-thai/)

## 4. ปลดล๊อคอุปกรณ์ Android เพื่อใช้ทดสอบแอพ

- [วิธีปลดล๊อคอุปกรณ์ Android เพื่อใช้ทดสอบแอพ](https://nextflow.in.th/2014/enable-android-developer-option/)

## 5. ติดตั้ง Git client

1. [เลือกดาวน์โหลดตัว installer จากเว็บนี้](https://git-scm.com/downloads) และทำการติดตั้งให้เรียบร้อย 
2. รันคำสั่งด้านล่าง เพื่อกำหนดชื่อ user สำหรับระบบ git เครื่องนี้ **(อย่าล่ืมใส่ชื่อตัวเองเป็นภาษาอังกฤษ ลงไปแทนที่ "FIRST_NAME LAST_NAME")**

```bash
git config --global user.name "FIRST_NAME LAST_NAME"
```

3. รันคำสั่งด้านล่าง เพื่อกำหนด email สำหรับระบบ git เครื่องนี้ **(อย่าล่ืมใส่อีเมลล์ของตัวเอง ลงไปแทนที่ "myemail@example.com")**

```bash
git config --global user.email "myemail@example.com"
```
