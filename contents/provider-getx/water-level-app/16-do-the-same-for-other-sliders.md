# ทำแบบเดียวกันสำหรับ Slider อื่นๆ

## 1. สร้างตัวแปรสำหรับ slider ที่เหลือ

```dart
// lib/pages/gate_calculator_page/gate_calculator_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

import '../../data/server_data.dart';

class GateCalculatorController extends GetxController {
  var resultText = "".obs;

  var isLoading = false.obs;
  var connect = Get.put(GetConnect());

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

  var u1Enabled = true.obs;
  
  // สร้างตัวแปรสำหรับ slider ที่เหลือ 
  var u2Enabled = true.obs;
  var u3Enabled = true.obs;
  var u4Enabled = true.obs;
  var u5Enabled = true.obs;

  void calculate() async {
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

    resultText.value = "";

    var body = {
      "lv_inp": lv_inp,
      "ttn_for_inp": ttn_for,
      "demand_irr": demand_irr,
      "snr_rrel": snr_rrel,
      "ttnsumrel": ttnsumrel,
      "u1": true,
      "u2": true,
      "u3": true,
      "u4": true,
      "u5": true,
      "u1s": u1Value.value,
      "u2s": u2Value.value,
      "u3s": u3Value.value,
      "u4s": u4Value.value,
      "u5s": u5Value.value
    };

    print(body);

    var response = await connect.post(
      'https://waterlevelapi.azurewebsites.net/level-api.php',
      body,
      contentType: 'application/json',
    );

    if (response.status.hasError) {
      print('Error: ${response.statusText}');
    } else {
      print('Success: ${response.bodyString}');
    }

    resultText.value += response.body["output"].toString();
  }

  Future<void> loadServerData() async {
    isLoading.value = true;

    await Future.delayed(Duration(seconds: 3));

    var response = await connect.get(
        "https://nextflowxegatx2023.blob.core.windows.net/server/server-data.json");
    print(response.bodyString);
    var serverData = ServerData.fromMap(response.body);

    snr_rrel = serverData.snrRrel ?? 0;
    snr_rrel_textCtrl.value.text = snr_rrel.toString();

    ttn_for = serverData.ttnFor ?? 0;
    ttn_for_textCtrl.value.text = ttn_for.toString();

    ttnsumrel = serverData.ttnsumrel ?? 0.0;
    ttnsumrel_textCtrl.value.text = ttnsumrel.toString();

    lv_inp = ttn_for;
    lv_inp_textCtrl.value.text = lv_inp.toString();

    demand_irr = serverData.ttnDemandIrr ?? 0.0;
    demand_irr_textCtrl.value.text = demand_irr.toString();

    isLoading.value = false;
  }
}


```

## 2. ทำค่าตัวแปรมาใช้กับ Unit และ Slider ที่เหลือ

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
    controller.loadServerData();

    return Scaffold(
      appBar: AppBar(
        title: Text('คำนวนประตูน้ำ'),
      ),
      body: Obx(() {
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
                Row(
                  children: [
                    Checkbox(
                      value: controller.u1Enabled.value,
                      onChanged: (bool? value) {
                        controller.u1Enabled.value = value!;
                      },
                    ),
                    Text('Unit 1 (${controller.u1Value.value})'),
                  ],
                ),
                Slider(
                  value: controller.u1Value.value,
                  min: 80,
                  max: 120,
                  divisions: 4,
                  label: controller.u1Value.value.toString(),
                  onChanged: controller.u1Enabled.value
                      ? (double value) {
                          controller.u1Value.value = value;
                        }
                      : null,
                ),

                // เพิ่ม checkbox และจัด layout ให้กับ unit ที่ 2
                Row(
                  children: [
                    Checkbox(
                      value: controller.u2Enabled.value,
                      onChanged: (bool? value) {
                        controller.u2Enabled.value = value!;
                      },
                    ),
                    Text('Unit 2 (${controller.u2Value.value})'),
                  ],
                ),
                // นำค่า u2Enabled มาใช้กับ onChanged ของ Slider
                Slider(
                  value: controller.u2Value.value,
                  min: 80,
                  max: 120,
                  divisions: 4,
                  label: controller.u2Value.value.toString(),
                  onChanged: controller.u2Enabled.value
                      ? (double value) {
                          controller.u2Value.value = value;
                        }
                      : null,
                ),
                // เพิ่ม checkbox และจัด layout ให้กับ unit ที่ 3
                Row(
                  children: [
                    Checkbox(
                      value: controller.u3Enabled.value,
                      onChanged: (bool? value) {
                        controller.u3Enabled.value = value!;
                      },
                    ),
                    Text('Unit 3 (${controller.u3Value.value})'),
                  ],
                ),
                // นำค่า u3Enabled มาใช้กับ onChanged ของ Slider
                Slider(
                  value: controller.u3Value.value,
                  min: 80,
                  max: 120,
                  divisions: 4,
                  label: controller.u3Value.value.toString(),
                  onChanged: controller.u3Enabled.value
                      ? (double value) {
                          controller.u3Value.value = value;
                        }
                      : null,
                ),
                // เพิ่ม checkbox และจัด layout ให้กับ unit ที่ 4
                Row(
                  children: [
                    Checkbox(
                      value: controller.u4Enabled.value,
                      onChanged: (bool? value) {
                        controller.u4Enabled.value = value!;
                      },
                    ),
                    Text('Unit 4 (${controller.u4Value.value})'),
                  ],
                ),
                // นำค่า u4Enabled มาใช้กับ onChanged ของ Slider
                Slider(
                  value: controller.u4Value.value,
                  min: 150,
                  max: 180,
                  divisions: 3,
                  label: controller.u4Value.value.toString(),
                  onChanged: controller.u4Enabled.value
                      ? (double value) {
                          controller.u4Value.value = value;
                        }
                      : null,
                ),
                // เพิ่ม checkbox และจัด layout ให้กับ unit ที่ 5
                Row(
                  children: [
                    Checkbox(
                      value: controller.u5Enabled.value,
                      onChanged: (bool? value) {
                        controller.u5Enabled.value = value!;
                      },
                    ),
                    Text('Unit 5 (${controller.u5Value.value})'),
                  ],
                ),
                // นำค่า u5Enabled มาใช้กับ onChanged ของ Slider
                Slider(
                  value: controller.u5Value.value,
                  min: 150,
                  max: 180,
                  divisions: 3,
                  label: controller.u5Value.value.toString(),
                  onChanged: controller.u5Enabled.value
                      ? (double value) {
                          controller.u5Value.value = value;
                        }
                      : null,
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
                SizedBox(
                  height: 16,
                ),
                Text(controller.resultText.value),
              ],
            ),
          ),
        );
      }),
    );
  }
}

```

## 3. นำค่าของ u1Enabled และตัวอื่นๆ ส่งไปให้ server 

```dart
// lib/pages/gate_calculator_page/gate_calculator_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

import '../../data/server_data.dart';

class GateCalculatorController extends GetxController {
  var resultText = "".obs;

  var isLoading = false.obs;
  var connect = Get.put(GetConnect());

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

  var u1Enabled = true.obs;
  var u2Enabled = true.obs;
  var u3Enabled = true.obs;
  var u4Enabled = true.obs;
  var u5Enabled = true.obs;

  void calculate() async {
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

    resultText.value = "";

    var body = {
      "lv_inp": lv_inp,
      "ttn_for_inp": ttn_for,
      "demand_irr": demand_irr,
      "snr_rrel": snr_rrel,
      "ttnsumrel": ttnsumrel,
      "u1": u1Enabled.value,
      "u2": u2Enabled.value,
      "u3": u3Enabled.value,
      "u4": u4Enabled.value,
      "u5": u5Enabled.value,
      "u1s": u1Value.value,
      "u2s": u2Value.value,
      "u3s": u3Value.value,
      "u4s": u4Value.value,
      "u5s": u5Value.value
    };

    print(body);

    var response = await connect.post(
      'https://waterlevelapi.azurewebsites.net/level-api.php',
      body,
      contentType: 'application/json',
    );

    if (response.status.hasError) {
      print('Error: ${response.statusText}');
    } else {
      print('Success: ${response.bodyString}');
    }

    resultText.value += response.body["output"].toString();
  }

  Future<void> loadServerData() async {
    isLoading.value = true;

    await Future.delayed(Duration(seconds: 3));

    var response = await connect.get(
        "https://nextflowxegatx2023.blob.core.windows.net/server/server-data.json");
    print(response.bodyString);
    var serverData = ServerData.fromMap(response.body);

    snr_rrel = serverData.snrRrel ?? 0;
    snr_rrel_textCtrl.value.text = snr_rrel.toString();

    ttn_for = serverData.ttnFor ?? 0;
    ttn_for_textCtrl.value.text = ttn_for.toString();

    ttnsumrel = serverData.ttnsumrel ?? 0.0;
    ttnsumrel_textCtrl.value.text = ttnsumrel.toString();

    lv_inp = ttn_for;
    lv_inp_textCtrl.value.text = lv_inp.toString();

    demand_irr = serverData.ttnDemandIrr ?? 0.0;
    demand_irr_textCtrl.value.text = demand_irr.toString();

    isLoading.value = false;
  }
}


```