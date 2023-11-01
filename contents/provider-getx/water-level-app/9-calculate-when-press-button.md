
# เริ่มการคำนวนเมื่อกดปุ่ม ตอนนี้ให้แสดงข้อมูลที่ได้จากช่องกรอกไปก่อน

## 1. สร้าง Function เพื่อเริ่มการคำนวน

```dart
// lib/pages/gate_calculator_page/gate_calculator_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class GateCalculatorController extends GetxController {
  var ttn_for = 0.0;
  var ttn_for_textCtrl = TextEditingController().obs;

  var lv_inp = 0.0;
  var lv_inp_textCtrl = TextEditingController().obs;

  var demand_irr = 0.0;
  var demand_irr_textCtrl = TextEditingController().obs;

  var snr_rrel = 0.0;
  var snr_rrel_textCtrl = TextEditingController().obs;

  var ttnsumrel = 0.0;
  var ttnsumrel_textCtrl = TextEditingController().obs;

  var dis_tol = 0.0;
  var dis_tol_textCtrl = TextEditingController().obs;

  // สร้าง Function เพื่อเริ่มการคำนวน
  calculate() {
    print('ttn_for: $ttn_for');
    print('lv_inp: $lv_inp');
    print('demand_irr: $demand_irr');
    print('snr_rrel: $snr_rrel');
    print('ttnsumrel: $ttnsumrel');
    print('dis_tol: $dis_tol');
  }
}

```


## 2. เริ่มการคำนวนเมื่อกดปุ่ม 

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
                  onPressed: () {

                    // เริ่มการคำนวนเมื่อกดปุ่ม    
                    controller.calculate();
                  },
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