# สร้าง class เก็บข้อมูล json

## 1. ติดตั้ง JSONtoDart Extension 

ทำการติดตั้ง Extension นี้ ลงใน Visual Studio Code [JSON to Dart Model](https://marketplace.visualstudio.com/items?itemName=hirantha.json-to-dart)

## 2. คัดลอก JSON 

ตัวอย่าง JSON อยู่ใน https://nextflowxegatx2023.blob.core.windows.net/server/server-data.json

```json 
{
    "snr_rfore": 171.83,
    "snr_rtail": 58.66,
	"snr_rhead": 113.17,
    "snr_rrel": 0.6016,
	"sumsnrrel": 0.8376,
	"sumsnrpump": 0.0,
    "ttn_for": 58.17,
    "ttn_stor": 43.6631,
    "ttn_rel": 0.0,
    "ttnsumrel": 0.0,
    "ttn_demand_irr": 3
}
```

ให้ทำการ copy JSON นี้ไว้

## 3. สร้าง Class 

1. ใน Visual Studio Code ให้เปิด Command Palette (Ctrl + P, Cmd + P) 
2. เลือกคำสั่ง **JSON to Dart: From Clipboard** และทำตามขั้นตอน
   1. ตั้งชื่อ base class: **ServerData**
   2. Implement Equality Operator: **No**
   3. You you want to use immutable class: **No**
   4. Implement `toString`: **No**
   5. Implement `copyWith`: **No**
   6. Suffix Json method: **Json**
   7. Implement Map JSON Method...: **No**
   8. สร้างโฟลเดอร์ `lib/data/` และเลือก directory นี้

จะได้ไฟล์มา 1 ไฟล์ 

> อย่าลืมเข้าไปแก้ data type ของตัวแปรให้เป็น double เพราะตัวแปลงจะอ่านและกำหนดจากข้อมูลตัวอย่างเท่านั้น 

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