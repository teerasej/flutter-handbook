
# สร้างหน้า Chat Page

ในหน้านี้เราจะสร้างหน้า Chat Room ที่ใช้สำหรับแสดงข้อความที่ได้รับจาก ChatGPT และส่งข้อความกลับไปยัง ChatGPT ได้

## 1. สร้างหน้า Chat Room

สร้างไฟล์ `lib/pages/chat/chat_page.dart` และเขียนโค้ดดังนี้

```dart
import 'package:flutter/material.dart';

class ChatPage extends StatelessWidget {
  const ChatPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Chat Page'),
      ),
    );
  }
}
```

## 2. นำ ChatPage มาแสดงเป็นหน้าแรก

แก้ไขไฟล์ `lib/main.dart` ให้เป็นดังนี้

```dart
import 'package:flutter/material.dart';
import 'package:nextflow_chatgpt/pages/chat/chat_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Nextflow ChatGPT',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      // แสดงหน้า ChatPage เป็นหน้าแรก
      home: ChatPage()
    );
  }
}
```

บันทึกไฟล์​และทดสอบพรีวิวใน Emulator หรือ Device ของเรา