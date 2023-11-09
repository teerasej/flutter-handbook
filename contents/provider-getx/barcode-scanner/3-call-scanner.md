
# เรียกใช้ Scanner


```dart
// lib/barcode_controller.dart

import 'package:get/get.dart';
import 'package:flutter_barcode_scanner/flutter_barcode_scanner.dart';

class BarcodeController extends GetxController {
  var _barcodeValue = "".obs;

  startScan() async {
    // เรียกใช้ตัวแสกน
    String barcodeScanResult = await FlutterBarcodeScanner.scanBarcode(
      // กำหนดสีเส้นแสกน
      '#FF0000',

      // ข้อความบนปุ่มยกเลิก
      'Cancel',

      // เปิด/ปิดใช้งานปุ่มไฟฉาย
      false,
      ScanMode.DEFAULT,
    );

    _barcodeValue.value = barcodeScanResult;
  }
}

```