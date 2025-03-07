
# เพิ่ม iOS Permission 

ตั้งแต่ iOS 10 เป็นต้นมา Apple ให้นักพัฒนาต้องกำหนดข้อความสำหรับการเข้าใช้งานกล้องถ่ายรูป ในไฟล์ **ios/Runner/Info.plist**

```xml
    ...
    <key>NSCameraUsageDescription</key>
    <string>Grant access to your camera to start scanning</string>
</dict>

```

ไม่งั้นแอพจะไม่สามารถเข้าใช้งานระบบกล้องได้

