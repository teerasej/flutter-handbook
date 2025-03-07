
# เรียกใช้ Scanner

## 1. สร้าง field สำหรับเก็บ barcode value ใน `BarcodeController`

```dart
// lib/barcode_controller.dart
import 'package:get/get.dart';

class BarcodeController extends GetxController {

  // สร้าง field สำหรับเก็บ barcode value
  // สังเกตว่าเราใช้ `.obs` ในการประกาศตัวแปร เพื่อให้ GetX สามารถตรวจจับการเปลี่ยนแปลงของตัวแปรได้
  var barcodeValue = "".obs;
}

```

## 2. เรียกใช้ MobileScanner widget

เปิดไฟล์ `lib/barcode_page.dart` เพื่อใช้ `MobileScanner` widget ในการ scan barcode จากนั้นบันทึกไฟล์ และทดสอบรันแอพพลิเคชั่น

```dart
// lib/barcode_page.dart
import 'package:flutter/material.dart';
import 'package:mobile_scanner/mobile_scanner.dart';

import 'package:get/get.dart';
import 'barcode_controller.dart';

class BarcodePage extends StatelessWidget {
  BarcodePage({super.key});
  
  void _handleBarcode(BarcodeCapture barcodes) {
    // แสดง barcode value ที่ได้จากการ scan
    print(barcodes.barcodes.firstOrNull?.displayValue ?? '')
  }

  @override
  Widget build(BuildContext context) {
    
    // เพิ่ม Expanded > MobileScanner ใน body ของ Scaffold

    return Scaffold(
      appBar: AppBar(title: const Text('Scan Barcode')),
      backgroundColor: Colors.black,
      body: SafeArea(
        child: Column(
          children: [
            Expanded(
              child: MobileScanner(
                onDetect: _handleBarcode,
              ),
            ),
          ],
        ),
      ),
    );
  }
}


```

## 3. เรียกใช้ BarcodeController ใน BarcodePage 

เพื่อให้สามารถ update ค่า barcode value ที่ได้จากการ scan ไปแสดงในหน้าจอ ให้เรียกใช้ `BarcodeController` ใน `BarcodePage` และ update ค่าใน `barcodeValue` ใน `_handleBarcode` method

จากนั้นบันทึกไฟล์ และทดสอบรันแอพพลิเคชั่น

```dart
// lib/barcode_page.dart
import 'package:flutter/material.dart';
import 'package:mobile_scanner/mobile_scanner.dart';

import 'package:get/get.dart';
import 'barcode_controller.dart';

class BarcodePage extends StatelessWidget {
  BarcodePage({super.key});

  // สร้าง instance ของ BarcodeController โดยใช้ Get.find()
  final BarcodeController barcodeController = Get.find();

  // ใช้ค่าจาก controller ในการแสดงผลบนหน้าจอ
  Widget _buildBarcode(String value) {

      // ถ้าไม่มีข้อมูลให้แสดงข้อความ 'Scan something!'
      if (value.isEmpty) {
        return const Text(
          'Scan something!',
          overflow: TextOverflow.fade,
          style: TextStyle(color: Colors.white),
        );
      }

      // ถ้ามีข้อมูลให้แสดงข้อมูลที่ได้จากการ scan
      return Text(
        value,
        overflow: TextOverflow.fade,
        style: const TextStyle(color: Colors.white),
      );
  }
  
  void _handleBarcode(BarcodeCapture barcodes) {
    // อัพเดทค่า barcode value ที่ได้จากการ scan ไปยัง controller
    barcodeController.barcodeValue.value = barcodes.barcodes.firstOrNull?.displayValue ?? '';
  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(title: const Text('Scan Barcode')),
      backgroundColor: Colors.black,
      body: SafeArea(
        child: Column(
          children: [
            Expanded(
              child: MobileScanner(
                onDetect: _handleBarcode,
              ),
            ),
            // แสดง barcode value ที่ได้จากการ scan
            Center(child: Obx(() => _buildBarcode(barcodeController.barcodeValue.value))),
          ],
        ),
      ),
    );
  }
}


```

## 4. นำค่าที่ได้จากการ scan ไปใช้งานใน HomePage 

เรียกใช้ `BarcodeController` ใน `HomePage` และแสดงค่าที่ได้จากการ scan บนหน้าจอ 

```dart
// lib/home_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

import 'barcode_controller.dart';

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
                Get.toNamed('/scan');
              },
              child: Text('Scan Barcode'),
            ),

            // แสดง barcode value ที่ได้จากการ scan
            Obx(() => Text(controller.barcodeValue.value),)
          ],
        ),
      ),
    );
  }
}

```
