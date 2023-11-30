
# สร้าง method สำหรับ save และ load ข้อมูล

```dart
// lib/barcode_controller.dart

import 'package:get/get.dart';
import 'package:flutter_barcode_scanner/flutter_barcode_scanner.dart';
import 'package:get_storage/get_storage.dart';

class BarcodeController extends GetxController {
  var barcodeValue = "".obs;

  startScan() async {
    String barcodeScanResult = await FlutterBarcodeScanner.scanBarcode(
      '#FF0000',
      'Cancel',
      false,
      ScanMode.DEFAULT,
    );

    barcodeValue.value = barcodeScanResult;
  }

 // สร้าง method สำหรับ save ข้อมูล barcode ลง storage
  saveBarcode() {
    var storage = GetStorage();
    storage.write('barcode', barcodeValue.value);

    // snack bar show text
    Get.snackbar(
      'Save Barcode',
      'Save Barcode value to storage',
      snackPosition: SnackPosition.BOTTOM,
    );
  }

  // สร้าง method สำหรับ load ข้อมูล barcode จาก storage
  loadBarcode() {
    var storage = GetStorage();
    barcodeValue.value = storage.read('barcode');

    // snack bar show text
    Get.snackbar(
      'Load Barcode',
      'Load Barcode value from storage',
      snackPosition: SnackPosition.BOTTOM,
    );
  }
}

```