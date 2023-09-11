
# สร้างตัวแทนข้อมูลของข่าวด้วย Class

1. สร้างไฟล์ **lib/news_data.dart**
2. เขียนโค้ดตามด้านล่าง 

```dart

// ตั้งชื่อข้อมูลก้อนนี้ว่า NewsData
class NewsData {

  // กำหนดให้ NewsData เก็บข้อมูลชื่อ content ไว้ เราเรียกอีกชื่อว่า property หรือ content property ของ NewsData
  String content = "...";

  // ถ้ามีการเรียกใช้ NewsData ที่ไหนในแอพ ต้องกำหนด string เพื่อเป็นข้อมูลให้ content property
  NewsData(this.content);
}

```