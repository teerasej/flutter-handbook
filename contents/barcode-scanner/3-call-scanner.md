
# เรียกใช้ Scanner


```dart
// lib/main.dart
import 'package:flutter_barcode_scanner/flutter_barcode_scanner.dart';


floatingActionButton: FloatingActionButton(
    onPressed: () async {

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

        // สั่ง setState เพื่อ rebuild widget
        setState(() {
        _result = barcodeScanResult;
        });
    },
    child: Icon(Icons.qr_code_scanner),
),
```