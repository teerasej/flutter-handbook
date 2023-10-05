
# สร้าง model ด้วย JSONtoDart

## 1. ติดตั้ง JSONtoDart Extension 

ทำการติดตั้ง Extension นี้ [JSON to Dart Model](https://marketplace.visualstudio.com/items?itemName=hirantha.json-to-dart)

## 2. คัดลอก JSON 

ตัวอย่าง JSON อยู่ใน https://651d740c44e393af2d59d2b4.mockapi.io/api/profiles 

ให้ทำการ copy JSON นี้ไว้

## 3. สร้าง Model Class 

1. ใน Visual Studio Code ให้เปิด Command Palette (Ctrl + P, Cmd + P) 
2. เลือกคำสั่ง **JSON to Dart: From Clipboard** และทำตามขั้นตอน
   1. ตั้งชื่อ base class: **ProfileModel**
   2. Implement Equality Operator: **No**
   3. You you want to use immutable class: **No**
   4. Implement `toString`: **No**
   5. Implement `copyWith`: **No**
   6. Suffix Json method: **Json**
   7. Implement Map JSON Method...: **No**
   8. สร้างโฟลเดอร์ `lib/models/` และเลือก directory นี้

## 4. จะได้ไฟล์มา 1 ไฟล์

```dart
// lib/models/profile_model/profile_model.dart

import 'dart:convert';

class ProfileModel {
  DateTime? createdAt;
  String? name;
  String? avatar;
  String? phone;
  String? id;

  ProfileModel({
    this.createdAt,
    this.name,
    this.avatar,
    this.phone,
    this.id,
  });

  factory ProfileModel.fromMap(Map<String, dynamic> data) => ProfileModel(
        createdAt: data['createdAt'] == null
            ? null
            : DateTime.parse(data['createdAt'] as String),
        name: data['name'] as String?,
        avatar: data['avatar'] as String?,
        phone: data['phone'] as String?,
        id: data['id'] as String?,
      );

  Map<String, dynamic> toMap() => {
        'createdAt': createdAt?.toIso8601String(),
        'name': name,
        'avatar': avatar,
        'phone': phone,
        'id': id,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [ProfileModel].
  factory ProfileModel.fromJson(String data) {
    return ProfileModel.fromMap(json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [ProfileModel] to a JSON string.
  String toJson() => json.encode(toMap());
}

```
