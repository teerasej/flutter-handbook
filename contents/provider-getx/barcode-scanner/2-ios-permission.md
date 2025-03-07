
# เพิ่ม iOS Permission 

ตั้งแต่ iOS 10 เป็นต้นมา Apple ให้นักพัฒนาต้องกำหนดข้อความสำหรับการเข้าใช้งานกล้องถ่ายรูป ในไฟล์ **info.plist**

```xml
// ios/Runner/Info.plist
    ...
    <key>NSCameraUsageDescription</key>
    <string>This app needs camera access to scan QR codes</string>
    
    <key>NSPhotoLibraryUsageDescription</key>
    <string>This app needs photos access to get QR code from photo library</string>
</dict>

```

ไม่งั้นแอพจะไม่สามารถเข้าใช้งานระบบกล้องได้

