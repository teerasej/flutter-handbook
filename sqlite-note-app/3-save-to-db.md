
# การบันทึกข้อมูล


## 1. Import package

เราจะมีการเรียกใช้ package หลายตัวในการอ้างอิงตำแหน่งไฟล์ฐานข้อมูล และการทำงานกับฐานข้อมูลโดยตรง

```dart
// lib/main.dart
import 'package:sqflite/sqflite.dart';
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart';
import 'dart:io';
```

## 2. สร้างการเชื่อมต่อไปที่ไฟล์ฐานข้อมูล

เพราะการเรียกใช้ฐานข้อมูล อาจจะเกิดขึ้นมาที่ไหนในแอพก็ได้

เราจะทำการเขียน method ขึ้นมา 1 ตัว ซึ่งจะทำการเชื่อมต่อไปที่ไฟล์ Database และคืนค่ากลับมาเป็นตัวแปรที่อ้างอิงไฟล์ฐานข้อมูลดังกล่าว 


```dart
Future<Database> createConnection() async {
    // อ้างอิงถึงที่อยู่โฟลเดอร์สำหรับเก็บไฟล์ภายในแอพของระบบนั้นๆ (iOS และ Android จะไม่เหมือนกัน)
    Directory documentsDirectory = await getApplicationDocumentsDirectory();

    // นำที่อยู่โฟลเดอร์สำหรับเก็บไฟล์ มาต่อกับชื่อไฟล์ฐานข้อมูล Nextflow.db เพื่ออ้างอิงถึงที่อยู่ของไฟล์ฐานข้อมูล
    // เช่นถ้าที่อยู่โฟลเดอร์คือ app/data/appdoc/
    // เราจะได้ที่อยู่ของไฟล์ฐานข้อมูลเป็น app/data/appdoc/Nextflow.db
    String path = join(documentsDirectory.path, "Nextflow.db");

    // สั่งเปิดการเชื่อมต่อ เราจะได้ future มาจากขั้นตอนนี้
    var connectionFuture = await openDatabase(
      path,
      version: 1,
      onOpen: (db) {},
      // เราสามารถสร้าง function ที่จะทำงานหลังการสร้างไฟล์ฐานข้อมูลเสร็จสิ้นได้ 
      onCreate: (Database db, int version) async {

        // ในที่นี้เรารัน SQL เพื่อสร้าง table สำหรับเก็บข้อมูลชื่อ Note ซึ่งจะมีคอลัมภ์ชื่อ id และ text สำหรับเก็บข้อมูล
        await db.execute("CREATE TABLE Note ("
            "id INTEGER PRIMARY KEY AUTOINCREMENT,"
            "text TEXT"
            ")");
      },
    );

    // คืน future ออกไปสำหรับส่วนที่เป็นตัวแทน database
    return connectionFuture;
  }
```

## 3. บันทึกข้อมูลลงไฟล์ฐานข้อมูล

ัสังเกตว่าจะมีปุ่ม TextButton อยู่บรรทัดที่ 63 ซึ่งการกดปุ่มนี้ จะเป็นการส่งค่าเข้าไปใน method ชื่อ `saveNewNote()`

```dart
TextButton(
    onPressed: () {
    var message = messageController.text;

    // เช็คข้อความว่าง ไม่ให้ถูกบันทึกลง database
    if (message != null && message.isNotEmpty) {
        saveNewNote(message);
    }
    },
    child: Text('บันทึก'),
),
```

ดังนั้นเราจะเอาข้อความที่ได้ บันทึกลงไปในฐานข้อมูล ภายใน method `saveNewNote()`

```dart
void saveNewNote(String text) async {

    // เปิดการเชื่อมต่อ เพื่อได้ส่วนอ้างอิงไฟล์ฐานข้อมูล
    var db = await createConnection();

    // เขียน SQL เพื่อเพิ่มข้อมูลลง Database ส่วนที่กำกับด้วยเครื่องหมาย ? จะถูกแทนที่ด้วยค่าตัวแปร ตอนทำงาน
    var sql = "INSERT INTO Note (text) VALUES (?)";

    // ใช้ rawInsert ของ Database เพื่อรันคำสั่ง SQL และแทรกค่าตัวแปรลงไปใน ส่วนที่กำกับด้วยเครื่องหมาย ? ในคำสั่ง SQL
    var res = await db.rawInsert(sql, [text]);

    setState(() {
      // สั่งเคลียร์ช่องข้อความ
      messageController.text = "";
    });
  }
```

onPressed: () async {

    var location = Location();
    var currentLocation = await location.getLocation();

    var locationText = '${currentLocation['latitude']} ${currentLocation['longitude']}';

    print(locationText);
    setState(() {
        _currentLocationText = locationText;
    });
},
```