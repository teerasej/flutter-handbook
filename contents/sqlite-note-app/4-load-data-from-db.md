
# โหลดข้อมูลจากไฟล์ Database 

## 1. โหลดข้อมูล และแปลงเป็น List 

เราจะทำการสร้าง method ชื่อ `loadNote()` เพื่ออ่านข้อมูลที่บันทึกขึ้นมาจากไฟล์ฐานข้อมูล 


```dart
// method นี้จะคืนค่าเป็น List<String> เพราะข้อมูลที่ใช้งานในที่นี้เป็นข้อความธรรมดา อาจจะปรับเปลี่ยนได้ตามความเหมาะสม
Future<List<String>> loadNote() async {

    // เชื่อมต่อฐานข้อมูล
    final db = await createConnection();

    // db.query() สามารถกำหนดชื่อ table ที่ต้องการอ่านข้อมูลได้ มีค่าเทียบเท่า SQL: SELECT * FROM Note
    var res = await db.query("Note");

    List<String> resultList;

    // เช็คว่ามีข้อมูลจากฐานข้อมูลหรือไม่ 
    if (res.isNotEmpty) {

      // แปลงข้อมูลในรูปแบบคอลัมภ์ที่ละแถว (row) ให้เป็นข้อมูลที่สามารถนำไปใช้งานได้ในรูปแบบ List
      resultList = res.map((row) {
        var result = row['text'].toString();
        return result;
      }).toList();
    } else {
      // ถ้าไม่มีข้อมูลจากฐานข้อมูล ให้กำหนดผลลัพธ์จากการเรียกใช้ method นี้เป็น array เปล่าๆ 
      resultList = [];
    }

    return resultList;
  }
```



## 2. กำหนดให้โหลดข้อมูลหลังจากบันทึกข้อมูลเสร็จแล้ว 

หลังจากบันทึกข้อมูลเสร็จใน method `saveNewNote()` เราจะสั่งโหลดข้อมูลมาเก็บไว้ใน property ก่อนที่จะถูกนำไปใช้ในการแสดงผล ListView

```dart
void saveNewNote(String text) async {
    var db = await createConnection();
    var sql = "INSERT INTO Note (text) VALUES (?)";
    var res = await db.rawInsert(sql, [text]);

    // สั่งโหลดข้อมูลมาเก็บไว้ใน property ซึ่งจะถูกดึงมาใช้ใน ListView ตอน Widget ทำการ build ตัวเองใหม่
    noteList = await loadNote();

    setState(() {
      messageController.text = "";
    });
}
```

## 3. กำหนดให้โหลดข้อมูลตอนที่เปิดหน้า Home Page

ในที่นี้เราจะสั่งโหลดข้อมูลจาก constructor ของ Widget `MyHomePage` 

```dart
_MyHomePageState() {
    this.loadRecentMessage();
}

// การทำงานแบบ async ไม่สามารถเรียกใช้โดยตรงผ่าน constructor ได้ เลยมาสร้างแยกไว้ต่างหาก
void loadRecentMessage() async {
    // โหลดข้อมูลมาเก็บไว้ใน property
    noteList = await loadNote();
    // และสั่งให้ build ตัว widget อีกครั้ง
    setState(() {});
}
```