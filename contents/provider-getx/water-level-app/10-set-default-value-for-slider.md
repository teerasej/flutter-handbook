
# กำหนดค่าเริ่มต้นให้ slider และแสดงค่าใน console

## 1. กำหนดค่าของ Unit ใน controller


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

  // กำหนดค่าเริ่มต้นให้ slider
  var u1Value = 80.0.obs;
  var u2Value = 80.0.obs;
  var u3Value = 80.0.obs;
  var u4Value = 150.0.obs;
  var u5Value = 150.0.obs;

  calculate() {
    print('ttn_for: $ttn_for');
    print('lv_inp: $lv_inp');
    print('demand_irr: $demand_irr');
    print('snr_rrel: $snr_rrel');
    print('ttnsumrel: $ttnsumrel');
    print('dis_tol: $dis_tol');

    // แสดงค่าที่ได้จาก slider ใน console
    print('u1: $u1Value');
    print('u2: $u2Value');
    print('u3: $u3Value');
    print('u4: $u4Value');
    print('u5: $u5Value');
  }
}


```

## 2. ผูกค่าเริ่มต้นเข้ากับ slider แต่ละตัว

เราจะมีการใช้ `Obx()` widget ในการทำให้ slider แสดงค่าที่เปลี่ยนไปตามที่เรากำหนดใน controller


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
      // ครอบ Obx เพื่อให้เกิดการ re-render ใหม่ ทุกครั้งที่ค่าตัวแปร obs มีการเปลี่ยน
      body: Obx(() {
        return SingleChildScrollView(
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
                // แสดงค่า slider แรกในชื่อ Unit
                Text('Unit 1 (${controller.u1Value.value})'),
                Slider(
                  
                  // กำหนดค่าเริ่มต้นให้ slider แรก ด้วยตัวแปร u1Value ใน controller
                  value: controller.u1Value.value,
                  
                  min: 80,
                  max: 120,
                  divisions: 4,

                  // กำหนดค่าให้แสดงใน slider ด้วยค่า u1Value ใน controller เมื่อ slider มีการเลื่อน
                  label: controller.u1Value.value.toString(),

                  onChanged: (double value) {
                    // กำหนดค่าให้ตัวแปร u1Value ใน controller เมื่อ slider มีการเลื่อน
                    controller.u1Value.value = value;
                  },
                ),
                
                // แสดงค่า slider ในชื่อ Unit และตัว slider
                 Text('Unit 2 (${controller.u2Value.value})'),
                Slider(
                  value: controller.u2Value.value,
                  min: 80,
                  max: 120,
                  divisions: 4,
                  label: controller.u2Value.value.toString(),
                  onChanged: (double value) {
                    controller.u2Value.value = value;
                  },
                ),

                // แสดงค่า slider ในชื่อ Unit และตัว slider
                Text('Unit 3 (${controller.u3Value.value})'),
                Slider(
                  value: controller.u3Value.value,
                  min: 80,
                  max: 120,
                  divisions: 4,
                  label: controller.u3Value.value.toString(),
                  onChanged: (double value) {
                    controller.u3Value.value = value;
                  },
                ),

                // แสดงค่า slider ในชื่อ Unit และตัว slider
                Text('Unit 4 (${controller.u4Value.value})'),
                Slider(
                  value: controller.u4Value.value,
                  min: 150,
                  max: 180,
                  divisions: 3,
                  label: controller.u4Value.value.toString(),
                  onChanged: (double value) {
                    controller.u4Value.value = value;
                  },
                ),
                
                // แสดงค่า slider ในชื่อ Unit และตัว slider
                Text('Unit 5 (${controller.u5Value.value})'),
                Slider(
                  value: controller.u5Value.value,
                  min: 150,
                  max: 180,
                  divisions: 3,
                  label: controller.u5Value.value.toString(),
                  onChanged: (double value) {
                    controller.u5Value.value = value;
                  },
                ),
                SizedBox(
                  height: 16,
                ),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: () {
                      controller.calculate();
                    },
                    child: const Text('คำนวน'),
                  ),
                ),
              ],
            ),
          ),
        );
      }),
    );
  }
}


```