# สร้าง function สำหรับโหลดข้อมูลจาก server

## 1. สร้าง function สำหรับโหลดข้อมูลจาก server

```dart
// lib/pages/gate_calculator_page/gate_calculator_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class GateCalculatorController extends GetxController {

  //  สร้างตัวแปร isLoading ไว้เก็บค่าว่ากำลังโหลดหรือไม่
  var isLoading = false.obs;

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

    print('u1: $u1Value');
    print('u2: $u2Value');
    print('u3: $u3Value');
    print('u4: $u4Value');
    print('u5: $u5Value');
  }

  // สร้าง function สำหรับโหลดข้อมูลจาก server
  Future<void> loadServerData() async {
    
    // กำหนดค่า isLoading เป็น true เพื่อให้แสดงตัวโหลด
    isLoading.value = true;

    // ใช้ Future.delayed ในการ delay การทำงาน 3 วินาที
    await Future.delayed(Duration(seconds: 3));

    // กำหนดค่า isLoading เป็น false เพื่อให้แสดงข้อมูล
    isLoading.value = false;
  }
}

```

## 2. สร้างเงื่อนไขเช็คว่าควรแสดงตัวโหลดหรือไม่ 

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

    // เรียกใช้ function สำหรับโหลดข้อมูลจาก server ซึ่งจะทำให้ตัวแปร isLoading มีการเปลี่ยนค่า และส่งผลต่อการแสดงผลในหน้าจอ
    controller.loadServerData();

    return Scaffold(
      appBar: AppBar(
        title: Text('คำนวนประตูน้ำ'),
      ),
      body: Obx(() {

        // สร้างเงื่อนไขเช็คว่าควรแสดงตัวโหลดหรือไม่    
        if (controller.isLoading.value) {
          return Center(
            child: CircularProgressIndicator(),
          );
        }

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
                Text('Unit 1 (${controller.u1Value.value})'),
                Slider(
                  value: controller.u1Value.value,
                  min: 80,
                  max: 120,
                  divisions: 4,
                  label: controller.u1Value.value.toString(),
                  onChanged: (double value) {
                    controller.u1Value.value = value;
                  },
                ),
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