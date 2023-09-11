
# แสดง Note ของผู้ใช้

หน้า Note Page จะเป็นส่วนที่ดึงข้อมูลของผู้ใช้ปัจจุบันมาแสดง ดังนั้นการขอข้อมูลจาก Web API ต้องมีการแนบ Token ไปด้วย

## 1. กำหนด Token ให้กับตัวส่ง Request 

ในที่นี่เราจะดึง dio มาใช้งาน และกำหนด token ลงไปในส่วนของ Header

```dart
class _NotePageState extends State<NotePage> {
  @override
  Widget build(BuildContext context) {
    
    // เรียกใช้งาน instance ของ dio
    var dio = Connection.getDio();

    // กำหนด token ลงไปในส่วนของ header 'Authorization'
    dio.options.headers['Authorization'] = "Bearer ${TokenManager.userToken}";

    //...

  }
}
```

## 2. ใช้ FutureBuilder จัดการแสดง UI ตามสถานะการขอข้อมูล 

เนื่องจากข้อมูลที่ได้

```dart
Widget build(BuildContext context) {
    
    var dio = Connection.getDio();
    dio.options.headers['Authorization'] = "Bearer ${TokenManager.userToken}";

    return Scaffold(
      appBar: AppBar(
        title: Text('บันทึก'),
      ),
      body: FutureBuilder(
        // ส่ง request ไปตาม URL ที่กำหนด
        future: dio.get('/notes'),
        builder: (BuildContext context, AsyncSnapshot<Response> snapshot) {

          // ถ้าได้ข้อมูลแล้ว ก็เอามาแสดงเป็น list view
          if (snapshot.connectionState == ConnectionState.done) {

            // ข้อมูลที่ได้กลับมาจาก Future ใดๆ ก็ตาม จะอยู่ใน snapshot.data
            List items = snapshot.data.data;

            return ListView.builder(
              itemCount: items.length,
              itemBuilder: (BuildContext context, int index) {
                var item = items[index];

                return ListTile(
                  title: Text(item['message']),
                );
              },
            );
          }

          return LinearProgressIndicator();
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () async {

          // เปิดหน้าแอพสำหรับสร้าง message ใหม่
          var isNoteCreated = await Navigator.push(
            context,
            MaterialPageRoute(builder: (BuildContext context) {
              return NoteCreatePage();
            }),
          );

          // ถ้าค่าที่ส่งจากมา Navigator.pop() เป็น true ให้สั่ง setState()
          if (isNoteCreated) {
            setState(() {});
          }
        },
        child: Icon(Icons.add),
      ),
    );
  }
```


## ไฟล์เต็ม

```dart
// lib/pages/note_page.dart
import 'package:dio/dio.dart';
import 'package:flutter/material.dart';
import 'package:nextflow_note_client_with_authen/connection.dart';
import 'package:nextflow_note_client_with_authen/pages/note_create_page.dart';
import 'package:nextflow_note_client_with_authen/token_manager.dart';

class _NotePageState extends State<NotePage> {
  @override
  Widget build(BuildContext context) {
    var dio = Connection.getDio();
    dio.options.headers['Authorization'] = "Bearer ${TokenManager.userToken}";

    return Scaffold(
      appBar: AppBar(
        title: Text('บันทึก'),
      ),
      body: FutureBuilder(
        future: dio.get('/notes'),
        builder: (BuildContext context, AsyncSnapshot<Response> snapshot) {
          if (snapshot.connectionState == ConnectionState.done) {
            List items = snapshot.data.data;

            return ListView.builder(
              itemCount: items.length,
              itemBuilder: (BuildContext context, int index) {
                var item = items[index];

                return ListTile(
                  title: Text(item['message']),
                );
              },
            );
          }

          return LinearProgressIndicator();
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () async {
          var isNoteCreated = await Navigator.push(
            context,
            MaterialPageRoute(builder: (BuildContext context) {
              return NoteCreatePage();
            }),
          );

          if (isNoteCreated) {
            setState(() {});
          }
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```