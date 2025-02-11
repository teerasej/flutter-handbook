
# App Icon 

## การเปลี่ยน App Icon ใน Flutter

ใน Flutter สามารถเปลี่ยน App Icon ได้ง่ายๆ โดยการใช้งาน [`flutter_launcher_icons` package](https://pub.dev/packages/flutter_launcher_icons) ซึ่งเป็น package ที่ช่วยในการสร้าง App Icon สำหรับ iOS และ Android โดยอัตโนมัติ

### 1. การติดตั้ง

A. รันคำสั่งต่อไปนี้ใน Terminal หรือ Command Prompt จากใน path ของโปรเจค เพื่อเพิ่ม `flutter_launcher_icons` package ลงไปในโปรเจค

```bash
flutter pub add flutter_launcher_icons
```

B. เพิ่ม `flutter_launcher_icons` ใน `pubspec.yaml`

```yaml
dependencies:
  flutter_launcher_icons: ^0.14.3
```

### 2. การใช้งาน

1. รันคำสั่งต่อไปนี้เพื่อสร้างไฟล์ `flutter_launcher_icons.yaml` ใน root ของโปรเจค

```bash
dart run flutter_launcher_icons:generate
```

2. แก้ไขไฟล์ `flutter_launcher_icons.yaml` จาก template ที่ระบบสร้างให้เป็นดังนี้

```yaml
# flutter pub run flutter_launcher_icons
flutter_launcher_icons:
  image_path: "assets/icon/icon.png"

  android: "ic_launcher"
  # image_path_android: "assets/icon/icon.png"
  min_sdk_android: 21 # android min sdk min:16, default 21
  # adaptive_icon_background: "assets/icon/background.png"
  # adaptive_icon_foreground: "assets/icon/foreground.png"
  # adaptive_icon_monochrome: "assets/icon/monochrome.png"

  ios: true
  # image_path_ios: "assets/icon/icon.png"
  remove_alpha_channel_ios: true
  # image_path_ios_dark_transparent: "assets/icon/icon_dark.png"
  # image_path_ios_tinted_grayscale: "assets/icon/icon_tinted.png"
  # desaturate_tinted_to_grayscale_ios: true
```

3. สร้างโฟลเดอร์ `assets/icon` ใน root ของโปรเจค และเพิ่มไฟล์ `icon.png` ขนาด 1024x1024 px โดย[สามารถดาวน์โหลดจากที่นี่ไปใช้งานได้](https://user-images.githubusercontent.com/85179/113394518-99e82b00-93c2-11eb-9193-c091d6ecfba6.png)

4. รันคำสั่งต่อไปนี้เพื่อสร้าง App Icon สำหรับ iOS และ Android

```bash
flutter pub get
dart run flutter_launcher_icons
```

5. เช็คไฟล์ในฝั่ง android ตามที่อยู่ด้านล่าง 

```
android/app/src/main/res/mipmap-hdpi/ic_launcher.png
android/app/src/main/res/mipmap-mdpi/ic_launcher.png
android/app/src/main/res/mipmap-xhdpi/ic_launcher.png
android/app/src/main/res/mipmap-xxhdpi/ic_launcher.png
android/app/src/main/res/mipmap-xxxhdpi/ic_launcher.png
```

6. เช็คไฟล์ในฝั่ง ios ตามที่อยู่ด้านล่าง 

```
ios/Runner/Assets.xcassets/AppIcon.appiconset
```

7. ทดสอบรันแอพพลิเคชั่นใหม่