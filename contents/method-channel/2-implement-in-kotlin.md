

# implement การเรียกใช้ method จากใน Android (Kotlin)

เปิดไฟล์ **android/app/src/main/kotlin/com/example/nextflow_flutter_battery_native/MainActivity.kt**

```kotlin
package com.example.nextflow_flutter_battery_native

import io.flutter.embedding.android.FlutterActivity

import androidx.annotation.NonNull
import io.flutter.embedding.engine.FlutterEngine
import io.flutter.plugin.common.MethodChannel

import android.content.Context
import android.content.ContextWrapper
import android.content.Intent
import android.content.IntentFilter
import android.os.BatteryManager
import android.os.Build.VERSION
import android.os.Build.VERSION_CODES


class MainActivity: FlutterActivity() {

    // กำหนดชื่อ channel ที่ตรงกับที่เรียกจาก flutter
    private val CHANNEL = "th.in.nextflow/battery"

    override fun configureFlutterEngine(@NonNull flutterEngine: FlutterEngine) {
        super.configureFlutterEngine(flutterEngine)

        // เรียกใช้ method channel
        MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler {
            // Note: this method is invoked on the main thread.
            call, result ->

            // เช็คชื่อ method ที่มีการเรียก ถ้าตรงกับที่กำหนดก็รันโค้ดต่อได้เลย
            if (call.method == "getBatteryLevel") {

              // เรียกใช้ code native
              val batteryLevel = getBatteryLevel()
      
              if (batteryLevel != -1) {

                // ใช้ method success ส่งค่าที่เป็นจำนวนแบตเตอรี่กลับไปให้ flutter
                result.success(batteryLevel)
              } else {

                // ใช้ method error ส่ง error กลับไปให้ flutter
                result.error("UNAVAILABLE", "Battery level not available.", null)
              }
            } else {
              // ถ้าไม่ตรงก็ส่ง error กลับไปว่ายังไม่ได้ implement
              result.notImplemented()
            }
        }
    }

    private fun getBatteryLevel(): Int {
        val batteryLevel: Int
        if (VERSION.SDK_INT >= VERSION_CODES.LOLLIPOP) {
          val batteryManager = getSystemService(Context.BATTERY_SERVICE) as BatteryManager
          batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY)
        } else {
          val intent = ContextWrapper(applicationContext).registerReceiver(null, IntentFilter(Intent.ACTION_BATTERY_CHANGED))
          batteryLevel = intent!!.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100 / intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1)
        }
    
        return batteryLevel
      }
}

```