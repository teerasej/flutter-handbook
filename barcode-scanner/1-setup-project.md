
# Setup 

## 1. ดาวน์โหลดและเตรียมโปรเจค

- สามารถดาวน์โหลด zip ไฟล์จาก[ลิ้งค์นี้](https://github.com/teerasej/nextflow_flutter_scanner/tree/starter)
- หรือใช้คำสั่ง clone โปรเจคด้วย git ด้านล่างก็ได้ 

```bash
git clone -b stater https://github.com/teerasej/nextflow_flutter_scanner
```

เปิดโปรเจคแล้วรันคำสั่ง `flutter package get` ให้เรียบร้อย

> ถ้าเปิดบน Visual Studio Code ก็อย่าลืมสั่ง get package ให้เรียบร้อย


## 2. รายชื่อ Package

ลองตรวจดูไฟล์ pubspec.yaml จะเห็นรายชื่อ package สำหรับโปรเจคนี้

```yaml
dependencies:
  ...
  flutter_barcode_scanner: ^1.0.2
```

- **flutter_barcode_scanner** สำหรับเรียกใช้ตัวแสกน และอ่านค่ากลับมาใช้งาน

## 3. การตั้งค่าสำหรับระบบ iOS

สำหรับระบบ iOS เราจำเป็นต้องตั้งค่าในส่วน Xcode เพิ่มเติมดังนี้

### 3.1 กำหนดค่าใน Xcode 

จากโปรแกรม Visual Studio Code ให้คลิกขวาที่โฟลเดอร์ iOS และเลือกคำสั่ง **Open in Xcode**

เลือกโปรเจค Runner > Project > Runner > กำหนดค่า **iOS Deployment Target** เป็น **11**

เลือกโปรเจค Runner > Target > Runner > Build Settings > ค้นหาการตั้งค่าที่ชื่อ swift > กำหนดค่า **Swift Language Version** เป็น **5**

เสร็จแล้วให้ปิดโปรแกรม Xcode ลง 
 

### รันคำสั่ง pod install

จากโปรแกรม Visual Studio Code ให้คลิกขวาที่โฟลเดอร์ iOS และเลือกคำสั่ง **Open in integrated Terminal**

พิมพ์และรันคำสั่งด้านล่าง

```bash
pod install
```

จะขึ้นข้อความประมาณด้านล่าง (อาจจะแตกต่างไปตามแต่ละโปรเจคของเรา เช่นถ้าเรามีหลาย package ก็จะมีการแสดงรายชื่อ package ขึ้นมาในรายการ)

```bash
Analyzing dependencies
Downloading dependencies
Installing Flutter (1.0.0)
Installing flutter_barcode_scanner (1.0.2)
Generating Pods project
Integrating client project
Pod installation complete! There are 2 dependencies from the Podfile and 2 total pods installed.

[!] Automatically assigning platform `iOS` with version `11.0` on target `Runner` because no platform was specified. Please specify a platform for this target in your Podfile. See `https://guides.cocoapods.org/syntax/podfile.html#platform`.

[!] CocoaPods did not set the base configuration of your project because your project already has a custom config set. In order for CocoaPods integration to work at all, please either set the base configurations of the target `Runner` to `Target Support Files/Pods-Runner/Pods-Runner.profile.xcconfig` or include the `Target Support Files/Pods-Runner/Pods-Runner.profile.xcconfig` in your build configuration (`Flutter/Release.xcconfig`).
```

ให้สังเกตคำแนะนำที่ 2 (สัญลักษณ์ [!]) ให้ดี นี่คือคำแนะนำที่เราต้องทำในขั้นตอนต่อไป 

### กำหนด Cocoa Pods configuration ให้โปรเจคเราใน Xcode

จากโปรแกรม Visual Studio Code ให้คลิกขวาที่โฟลเดอร์ iOS และเลือกคำสั่ง **Open in Xcode**

ในที่นี้เราจะมีการกำหนด configuration ของโปรเจค จากปกติให้ไปใช้ของ Cocoa pod แทน

เลือกโปรเจค Runner > Project > Runner > Configuration > Debug > กำหนดเป็น **Pods-Runner.debug**


เลือกโปรเจค Runner > Project > Runner > Configuration > Debug > กำหนดเป็น **Pods-Runner.release**


เลือกโปรเจค Runner > Project > Runner > Configuration > Debug > กำหนดเป็น **Pods-Runner.profile**

เสร็จเรียบร้อยแล้ว ให้ปิด Xcode ลง