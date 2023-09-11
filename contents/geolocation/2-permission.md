
# กำหนด Permission

## Android 

ไม่ต้องทำอะไรเพิ่มเติม ส่วนของ Permission จะถูกเพิ่มให้อัตโนมัติ


## iOS 

เปิดไฟล์

**ios/Runner/Info.plist**

และเพิ่มข้อความ

```xml
    <key>NSLocationAlwaysUsageDescription</key>
    <string>Needed to access location</string>
    <key>NSLocationWhenInUseUsageDescription</key>
    <string>Needed to access location</string>
</dict>
```

- **NSLocationAlwaysUsageDescription**: คำขอเข้าถึงตำแหน่งตลอดเวลา (แม้ว่าซ่อนแอพไปแล้ว)
- **NSLocationWhenInUseUsageDescription**: คำขอเข้าถึงตำแหน่งตอนใช้งานแอพเท่านั้น 