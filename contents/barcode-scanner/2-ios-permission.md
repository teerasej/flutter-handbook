
# เพิ่ม iOS Permission 

ตั้งแต่ iOS 10 เป็นต้นมา Apple ให้นักพัฒนาต้องกำหนดข้อความสำหรับการเข้าใช้งานกล้องถ่ายรูป ในไฟล์ info.plist

```xml
// ios/Runner/Info.plist
    ...
    <key>NSCameraUsageDescription</key>
    <string>Camera permission is required for barcode scanning.</string>
</dict>

```

ไม่งั้นแอพจะไม่สามารถเข้าใช้งานระบบกล้องได้

