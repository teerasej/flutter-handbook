# เรียกข้อมูล json จาก server และกำหนดให้กับค่าตัวแปร


## 1. ให้แน่ใจว่ามีไฟล์ server_data.dart อยู่ในโฟลเดอร์ lib/data ตามนี้ 

```dart 
// lib/data/server_data.dart

import 'dart:convert';

class ServerData {
  double? snrRfore;
  double? snrRtail;
  double? snrRhead;
  double? snrRrel;
  double? sumsnrrel;
  double? sumsnrpump;
  double? ttnFor;
  double? ttnStor;
  double? ttnRel;
  double? ttnsumrel;
  double? ttnDemandIrr;

  ServerData({
    this.snrRfore,
    this.snrRtail,
    this.snrRhead,
    this.snrRrel,
    this.sumsnrrel,
    this.sumsnrpump,
    this.ttnFor,
    this.ttnStor,
    this.ttnRel,
    this.ttnsumrel,
    this.ttnDemandIrr,
  });

  factory ServerData.fromMap(Map<String, dynamic> data) => ServerData(
        snrRfore: (data['snr_rfore'] as num?)?.toDouble(),
        snrRtail: (data['snr_rtail'] as num?)?.toDouble(),
        snrRhead: (data['snr_rhead'] as num?)?.toDouble(),
        snrRrel: (data['snr_rrel'] as num?)?.toDouble(),
        sumsnrrel: (data['sumsnrrel'] as num?)?.toDouble(),
        sumsnrpump: data['sumsnrpump'] as double?,
        ttnFor: (data['ttn_for'] as num?)?.toDouble(),
        ttnStor: (data['ttn_stor'] as num?)?.toDouble(),
        ttnRel: data['ttn_rel'] as double?,
        ttnsumrel: data['ttnsumrel'] as double?,
        ttnDemandIrr: data['ttn_demand_irr'] as double?,
      );

  Map<String, dynamic> toMap() => {
        'snr_rfore': snrRfore,
        'snr_rtail': snrRtail,
        'snr_rhead': snrRhead,
        'snr_rrel': snrRrel,
        'sumsnrrel': sumsnrrel,
        'sumsnrpump': sumsnrpump,
        'ttn_for': ttnFor,
        'ttn_stor': ttnStor,
        'ttn_rel': ttnRel,
        'ttnsumrel': ttnsumrel,
        'ttn_demand_irr': ttnDemandIrr,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [ServerData].
  factory ServerData.fromJson(String data) {
    return ServerData.fromMap(json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [ServerData] to a JSON string.
  String toJson() => json.encode(toMap());
}


```

## 2. ส่ง request ไปที่ URL ที่เตรียมส่งค่า json กลับมา ไว้

```dart 
// lib/pages/gate_calculator_page/gate_calculator_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

import '../../data/server_data.dart';

class GateCalculatorController extends GetxController {
  var isLoading = false.obs;

  // สร้าง GetConnect ใช้งานกับการเรียกข้อมูลจาก server
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

  Future<void> loadServerData() async {
    isLoading.value = true;

    await Future.delayed(Duration(seconds: 3));

    // ใช้ GetConnect ในการเรียกข้อมูลจาก server
    var response = await connect.get(
        "https://nextflowxegatx2023.blob.core.windows.net/server/server-data.json");
    
    // แสดงข้อมูลที่ได้จาก server ใน console
    print(response.bodyString);

    // สร้าง instance ของ ServerData จากคลาส ServerData และใช้ method fromMap ในการแปลงข้อมูลจาก json ให้กลายเป็น object
    var serverData = ServerData.fromMap(response.body);

    // กำหนดค่าให้กับตัวแปรต่างๆ โดยใช้ค่าจาก server
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