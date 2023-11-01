# สร้าง controller สำหรับหน้าคำนวน 

## 1. สร้างไฟล์ controller

สร้างไฟล์ `lib/pages/gate_calculator_page/gate_calculator_controller.dart`

```dart
// lib/pages/gate_calculator_page/gate_calculator_controller.dart

import 'package:get/get.dart';

class GateCalculatorController extends GetxController {}

```

## 2. setup controller สำหรับใช้งานในหน้า GateCalculator

```dart
// lib/pages/gate_calculator_page/gate_calculator_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

import 'gate_calculator_controller.dart';

class GateCalculatorPage extends StatelessWidget {
    
  // ลบ const ออก
  GateCalculatorPage({super.key});

  // สร้าง controller ในระบบ GetX
  var controller = Get.put(GateCalculatorController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('คำนวนประตูน้ำ'),
      ),
      body: SingleChildScrollView(
        child: Padding(
          padding: const EdgeInsets.all(8.0),
          child: Column(
            children: [
              TextField(
                decoration: InputDecoration(
                  labelText: 'จำลองระดับน้ำเหนือเขื่อน (m MSL)',
                ),
                keyboardType: TextInputType.number,
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'ระดับน้ำที่ต้องการยกระดับ (m MSL)',
                ),
                keyboardType: TextInputType.number,
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'แผนระบายน้ำ (m MSL)',
                ),
                keyboardType: TextInputType.number,
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'Total Sum SNR Release (MCM)',
                ),
                keyboardType: TextInputType.number,
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'Total Sum TTN Release (MCM)',
                ),
                keyboardType: TextInputType.number,
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'ปั้มสูบกลับ (m MSL)',
                ),
                keyboardType: TextInputType.number,
              ),
              SizedBox(
                height: 16,
              ),
              Text('Unit 1 (80)'),
              Slider(
                value: 80,
                min: 80,
                max: 120,
                divisions: 4,
                onChanged: (double value) {
                  print(value);
                },
              ),
              Text('Unit 2 (80)'),
              Slider(
                value: 80,
                min: 80,
                max: 120,
                divisions: 4,
                onChanged: (double value) {
                  print(value);
                },
              ),
              Text('Unit 3 (80)'),
              Slider(
                value: 80,
                min: 80,
                max: 120,
                divisions: 4,
                onChanged: (double value) {
                  print(value);
                },
              ),
              Text('Unit 4 (150)'),
              Slider(
                value: 150,
                min: 150,
                max: 180,
                divisions: 3,
                onChanged: (double value) {
                  print(value);
                },
              ),
              Text('Unit 5 (150)'),
              Slider(
                value: 150,
                min: 150,
                max: 180,
                divisions: 3,
                onChanged: (double value) {
                  print(value);
                },
              ),
              SizedBox(
                height: 16,
              ),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {},
                  child: const Text('คำนวน'),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

```