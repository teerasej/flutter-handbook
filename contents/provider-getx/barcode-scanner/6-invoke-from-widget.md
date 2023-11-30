
# สร้างปุ่มเรียกใช้ method

```dart
// lib/home_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_flutter_getx_barcode_scanner/barcode_controller.dart';

class HomePage extends StatelessWidget {
  var controller = Get.put(BarcodeController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Scan Barcode'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                controller.startScan();
              },
              child: Text('Scan Barcode'),
            ),
            SizedBox(height: 16),
            Obx(() {
              return Text(
                "${controller.barcodeValue.value}",
                style: TextStyle(fontSize: 18),
              );
            }),

            // เพิ่มปุ่ม Save 
            SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                controller.saveBarcode();
              },
              child: Text('Save Barcode value'),
            ),

            // เพิ่มปุ่ม load
            SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                controller.loadBarcode();
              },
              child: Text('Load Barcode value'),
            ),
          ],
        ),
      ),
    );
  }
}

```