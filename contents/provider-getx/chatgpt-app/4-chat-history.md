
# สร้าง Chat History สำหรับแสดงข้อความโต้ตอบ

## 1. สร้าง ChatHistory Widget

สร้าง `lib/pages/chat/chat_history_component.dart` และเขียนโค้ดตามนี้

```dart
// lib/pages/chat/chat_history_component.dart

import 'package:flutter/material.dart';

class ChatHistory extends StatelessWidget {
  const ChatHistory({super.key});
  
  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.grey[200],
      child: ListView(
        children: [
          // Add chat messages here
        ],
      ),
    );
  }
}
```

## 2. นำ ChatHistory ไปใช้ใน ChatPage

แก้ไข `lib/pages/chat/chat_page.dart` ให้เป็นดังนี้

```dart
import 'package:flutter/material.dart';

import 'chat_history_component.dart';
import 'chat_input_component.dart';

class ChatPage extends StatelessWidget {
  const ChatPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Chat Page'),
      ),
      // ใช้ Column เพื่อให้ ChatHistory และ ChatInput อยู่ใน Column เดียวกัน
      body: Column(
        children: [
          // ChatHistory จะแสดงข้อความที่เคยส่งไปแล้ว
          ChatHistory(),
          // ChatInput จะใช้สำหรับส่งข้อความ
          ChatInput(),
        ],
      ),
    );
  }
}
```

บันทึกและรีเฟรชหน้าจอของ Simulator หรือ Emulator ของคุณ จะเห็นหน้าจอ Chat Page ที่มี Chat History (ที่ยังไม่มีอะไร) และ Chat Input อยู่ในหน้าเดียวกัน

