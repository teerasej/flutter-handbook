
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

- ถ้ายังไม่มีไฟล์ **lib/news_data.dart** ให้ทำการสร้างไฟล์ดังกล่าวขึ้นมา

1. เปิดเว็บ [quicktype.io](https://app.quicktype.io/) และกดปุ่ม Open Quick Type เพื่อเริ่มใช้งาน

<img width="1119" alt="2021-08-16_21-48-16" src="https://user-images.githubusercontent.com/85179/129587221-62b6ec67-13bf-47e5-b296-909458fcc110.png">


2. ด้านขวาเราจะตั้งชื่อ Class ว่า **NewsData** และเลือก **Source type** เป็น **JSON** 

<img width="640" alt="2021-08-16_22-01-39" src="https://user-images.githubusercontent.com/85179/129587119-82becf9a-2954-4726-972a-bb4054b0ec49.png">


3. คัดลอก JSON ที่ได้จากจากการ[เรียก Web API Route **/news**](../web-api-part/5-test-api.md) หรือจะใช้ตัวอย่างจากด้านล่างนี้ก็ได้ มาวางด้านล่างชื่อ Class

```json
[
    {
        "_id": "1234",
        "content": "abcdefg"
    }
]
```

<img width="640" alt="2021-08-16_22-01-39" src="https://user-images.githubusercontent.com/85179/129587166-bd01b337-e1c9-4034-b546-49698f6ff583.png">


4. QuickType จะสร้างโค้ดตัวแทนของ JSON ทางด้านขวา ซึ่งเราใช้ตัวเลือกด้านขวาสุด

   1. เลือกภาษา Dart
   2. กด **Copy Code** 

<img width="853" alt="2021-08-16_22-01-49" src="https://user-images.githubusercontent.com/85179/129586903-6584db48-394d-46ba-8ddc-2d6f08929df8.png">


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
