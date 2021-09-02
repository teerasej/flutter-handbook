
# implement การเรียกใช้ method จากใน iOS (Swift)


เปิดไฟล์ **ios/Runner/AppDelegate.swift**

```swift
import UIKit
import Flutter

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {

    // เข้าถึง controller ที่ Flutter implement ไว้เรื่องเชื่อมกับ method channel  
    let controller : FlutterViewController = window?.rootViewController as! FlutterViewController

    // อ้างอิง method channel ที่ชื่อต้องตรงกับที่เรียกใน Flutter
    let batteryChannel = FlutterMethodChannel(name: "th.in.nextflow/battery",
                                              binaryMessenger: controller.binaryMessenger)
    
    // กำหนดกลไกการเรียกใช้ method และส่ง result กลับไปที่ flutter
    batteryChannel.setMethodCallHandler({
      (call: FlutterMethodCall, result: @escaping FlutterResult) -> Void in
      // Note: this method is invoked on the UI thread.
      // Handle battery messages.

      // เช็คชื่อ method ที่เรียกจาก flutter 
      guard call.method == "getBatteryLevel" else {
        result(FlutterMethodNotImplemented)
        return
      }
      self.receiveBatteryLevel(result: result)
    })

    GeneratedPluginRegistrant.register(with: self)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }

  private func receiveBatteryLevel(result: FlutterResult) {
    let device = UIDevice.current
    device.isBatteryMonitoringEnabled = true
    if device.batteryState == UIDevice.BatteryState.unknown {

        // ส่ง error กลับไปให้ Flutter ถ้าไม่สามารถเรียกข้อมูล battery ได้
      result(FlutterError(code: "UNAVAILABLE",
                          message: "Battery info unavailable",
                          details: nil))
    } else {
    
       // ส่งค่ากลับไปให้ Flutter
      result(Int(device.batteryLevel * 100))
    }
  }
}


```