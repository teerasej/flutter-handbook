
# กำหนด TextEditingController เพื่อใส่ข้อความลงใน TextField

## 1. สร้างตัวแปร ประเภท TextEditingController เพื่อกำหนดข้อความใน TextField

```dart
// controller
// lib/pages/gate_calculator_page/gate_calculator_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class GateCalculatorController extends GetxController {
  var ttn_for = 0.0;

  // สร้างตัวแปร ประเภท TextEditingController เพื่อกำหนดข้อความใน TextField
  var ttn_for_textCtrl = TextEditingController().obs;

  var lv_inp = 0.0;

  // สร้างตัวแปร ประเภท TextEditingController เพื่อกำหนดข้อความใน TextField
  var lv_inp_textCtrl = TextEditingController().obs;

  var demand_irr = 0.0;

  // สร้างตัวแปร ประเภท TextEditingController เพื่อกำหนดข้อความใน TextField
  var demand_irr_textCtrl = TextEditingController().obs;

  var snr_rrel = 0.0;

  // สร้างตัวแปร ประเภท TextEditingController เพื่อกำหนดข้อความใน TextField
  var snr_rrel_textCtrl = TextEditingController().obs;

  var ttnsumrel = 0.0;

  // สร้างตัวแปร ประเภท TextEditingController เพื่อกำหนดข้อความใน TextField
  var ttnsumrel_textCtrl = TextEditingController().obs;

  var dis_tol = 0.0;

  // สร้างตัวแปร ประเภท TextEditingController เพื่อกำหนดข้อความใน TextField
  var dis_tol_textCtrl = TextEditingController().obs;
}

```

## 2. นำมากำหนดใหักับ TextField แต่ละช่อง




```dart
// lib/pages/gate_calculator_page/gate_calculator_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

import 'gate_calculator_controller.dart';

class GateCalculatorPage extends StatelessWidget {
  GateCalculatorPage({super.key});

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

                // นำ TextEditingController มากำหนดใหักับ TextField แต่ละช่อง
                controller: controller.ttn_for_textCtrl.value,

                keyboardType: TextInputType.number,
                onChanged: (value) {
                  controller.ttn_for = double.parse(value);
                },
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'ระดับน้ำที่ต้องการยกระดับ (m MSL)',
                ),

                // นำ TextEditingController มากำหนดใหักับ TextField แต่ละช่อง
                controller: controller.lv_inp_textCtrl.value,

                keyboardType: TextInputType.number,
                onChanged: (value) {
                  controller.lv_inp = double.parse(value);
                },
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'แผนระบายน้ำ (m MSL)',
                ),

                // นำ TextEditingController มากำหนดใหักับ TextField แต่ละช่อง
                controller: controller.demand_irr_textCtrl.value,

                keyboardType: TextInputType.number,
                onChanged: (value) {
                  controller.demand_irr = double.parse(value);
                },
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'Total Sum SNR Release (MCM)',
                ),

                // นำ TextEditingController มากำหนดใหักับ TextField แต่ละช่อง
                controller: controller.snr_rrel_textCtrl.value,

                keyboardType: TextInputType.number,
                onChanged: (value) {
                  controller.snr_rrel = double.parse(value);
                },
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'Total Sum TTN Release (MCM)',
                ),

                // นำ TextEditingController มากำหนดใหักับ TextField แต่ละช่อง
                controller: controller.ttnsumrel_textCtrl.value,

                keyboardType: TextInputType.number,
                onChanged: (value) {
                  controller.ttnsumrel = double.parse(value);
                },
              ),
              TextField(
                decoration: InputDecoration(
                  labelText: 'ปั้มสูบกลับ (m MSL)',
                ),

                // นำ TextEditingController มากำหนดใหักับ TextField แต่ละช่อง
                controller: controller.dis_tol_textCtrl.value,
                
                keyboardType: TextInputType.number,
                onChanged: (value) {
                  controller.dis_tol = double.parse(value);
                },
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