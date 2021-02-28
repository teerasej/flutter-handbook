

# สร้าง Note ใหม่ของผู้ใช้งานปัจจุบัน


## 1. ดึงข้อมูลจาก Form มาสร้างเป็น Object

```dart
// lib/pages/note_create_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"message": _message};

    }
}
```

## 2. เพิ่ม user token เข้าไปใน header ของ request

```dart
// lib/pages/note_create_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"message": _message};

        // เรียกใช้งาน instance ของ dio
        var dio = Connection.getDio();

        // กำหนด token ลงไปในส่วนของ header 'Authorization'
        dio.options.headers['Authorization'] = "Bearer ${TokenManager.userToken}";

    }
}
```

## 3. ส่ง POST Request ไปที่เว็บ API เพื่อลงชื่อเข้าใช้งาน

```dart
// lib/pages/note_create_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"message": _message};

        var dio = Connection.getDio();
        dio.options.headers['Authorization'] = "Bearer ${TokenManager.userToken}";
        
        // เรียกใช้ Dio.post() เพื่อส่ง post request และรับ response กลับจาก web api
        final response = await Connection.getDio().post(
            // url สำหรับลงชื่อเข้าใช้งาน
            '/notes',
            // ใช้ json.encode() เพื่อแปลงข้อมูลให้อยู่ในรูปแบบ JSON format
            data: json.encode(data),
        );

    }
}
```

## 3. เช็คค่าและจัดการ token

```dart
// lib/pages/note_create_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"email": _email, "password": _password};

        final response = await Connection.getDio().post(
            '/signin',
            data: json.encode(data),
        );

        // check ข้อมูลจาก Web API
        // ในที่นี้ถ้า statueCode เป็น 200 ถือว่าการสร้าง note ใหม่เสร็จสมบูรณ์
        // ให้เปิดกลับไปหน้า Note Page โดยส่งค่า true กลับไปเพื่อให้ Note Page setState ใหม่ตามกลไกที่วางเอาไว้
         if (response.statusCode == 200) {
            Navigator.pop(context, true);
        } else {
            // ถ้ามีบางอย่างผิดพลาด ก็แสดง pop up 
            showDialog(
                context: context,
                builder: (BuildContext context) {
                return AlertDialog(
                    title: Text('Opps...'),
                    actions: [
                    TextButton(
                        child: Text('close'),
                        onPressed: () {
                        Navigator.pop(context);
                        },
                    )
                    ],
                );
                },
            );
        }

    }
}
```

## ไฟล์เต็ม

```dart
// lib/pages/note_create_page.dart

import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:nextflow_note_client_with_authen/connection.dart';
import 'package:nextflow_note_client_with_authen/token_manager.dart';

class NoteCreatePage extends StatefulWidget {
  @override
  _NoteCreatePageState createState() => _NoteCreatePageState();
}

class _NoteCreatePageState extends State<NoteCreatePage> {
  final _formKey = GlobalKey<FormState>();

  String _message = '';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('New Note'),
      ),
      body: Container(
        padding: EdgeInsets.all(10),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text('Message:'),
              SizedBox(
                height: 10,
              ),
              TextFormField(
                validator: (String message) {
                  if (message.isEmpty) {
                    return 'Please fill in message';
                  }
                  return null;
                },
                onSaved: (String message) {
                  _message = message;
                },
              ),
              SizedBox(
                height: 10,
              ),
              SizedBox(
                width: double.infinity,
                child: RaisedButton(
                  color: Colors.blue,
                  textColor: Colors.white,
                  onPressed: () async {
                    if (_formKey.currentState.validate()) {
                      _formKey.currentState.save();

                      var data = {"message": _message};

                      var dio = Connection.getDio();
                      dio.options.headers['Authorization'] =
                          "Bearer ${TokenManager.userToken}";

                      final response = await dio.post(
                        '/notes',
                        data: json.encode(data),
                      );

                      if (response.statusCode == 200) {
                        Navigator.pop(context, true);
                      } else {
                        showDialog(
                          context: context,
                          builder: (BuildContext context) {
                            return AlertDialog(
                              title: Text('Opps...'),
                              actions: [
                                TextButton(
                                  child: Text('close'),
                                  onPressed: () {
                                    Navigator.pop(context);
                                  },
                                )
                              ],
                            );
                          },
                        );
                      }
                    }
                  },
                  child: Text('Save'),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}


```