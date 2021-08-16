
# สร้าง Class ข้อมูล จากตัวอย่างข้อมูล JSON ด้วย QuickType.io

โดยปกติเราควรมีการกำหนดโครงสร้างของข้อมูล JSON ที่จะรับส่งกันระหว่างแอพ กับ Web API อยู่แล้ว เช่น

```json
[
    {
        "_id": "1234",
        "content": "abcdefg"
    }
]
```

การนำข้อมูลที่ได้รับจาก Web API มาใช้งานใน Flutter เรามักต้องเขียน Class ที่เป็นตัวแทนของข้อมูลเอง 

แต่เราสามารถลดงานตรงนี้ได้โดยใช้[เว็บ quicktype.io](https://app.quicktype.io/)

## 1. ใช้งาน QuickType.io เพื่อสร้าง Class ข้อมูล

1. เปิดเว็บ [quicktype.io](https://app.quicktype.io/) และกดปุ่ม Open Quick Type เพื่อเริ่มใช้งาน
2. ด้านขวาเราจะตั้งชื่อ Class ว่า **NewsData** และเลือก **Source type** เป็น **JSON** 
3. คัดลอก JSON ที่ได้จากจากการ[เรียก Web API Route **/news**](../web-api-part/5-test-api.md) หรือจะใช้ตัวอย่างจากด้านล่างนี้ก็ได้ มาวางด้านล่างชื่อ Class

```json
[
    {
        "_id": "1234",
        "content": "abcdefg"
    }
]
```

4. QuickType จะสร้างโค้ดตัวแทนของ JSON ทางด้านขวา ซึ่งเราใช้ตัวเลือกด้านขวาสุด

   1. เลือกภาษา Dart
   2. กด **Copy Code** 

5. เปิดไฟล์​ **lib/news_data.dart** ให้เราวางโค้ดที่ copy มาลงไปแทนที่โค้ดเดิม


6. จากนั้นให้เราเพิ่ม keyword `required` ไว้ด้านหน้า parameter ของ construtor NewsData

```dart
class NewsData {
  NewsData({
    // เพิ่ม keyword required
    required this.id,
    // เพิ่ม keyword required
    required this.content,
  });
```

7. บันทึกไฟล์ให้เรียบร้อย

## ไฟล์ lib/news_data.dart ที่ปรับปรุงแล้ว

```dart 
// To parse this JSON data, do
//
//     final newsData = newsDataFromJson(jsonString);

import 'package:meta/meta.dart';
import 'dart:convert';

List<NewsData> newsDataFromJson(String str) =>
    List<NewsData>.from(json.decode(str).map((x) => NewsData.fromJson(x)));

String newsDataToJson(List<NewsData> data) =>
    json.encode(List<dynamic>.from(data.map((x) => x.toJson())));

class NewsData {
  NewsData({
    required this.id,
    required this.content,
  });

  String id;
  String content;

  factory NewsData.fromJson(Map<String, dynamic> json) => NewsData(
        id: json["_id"],
        content: json["content"],
      );

  Map<String, dynamic> toJson() => {
        "_id": id,
        "content": content,
      };
}

```