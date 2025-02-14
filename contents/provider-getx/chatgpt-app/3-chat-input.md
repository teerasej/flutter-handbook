
# สร้าง Chat Input สำหรับส่งข้อความ

ในส่วนนี้เราจะสร้าง `TextField` สำหรับส่งข้อความไปยัง Chat Room 

## 1. สร้าง ChatInput Widget

สร้าง `lib/pages/chat/chat_input_component.dart` และเขียนโค้ดตามนี้

```dart
import 'package:flutter/material.dart';

class ChatInput extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(8.0),
      color: Colors.white,
      child: Text('Chat Input'),
    );
  }
}
```

## 2. นำ ChatInput ไปใช้ใน ChatPage

แก้ไข `lib/pages/chat/chat_page.dart` ให้เป็นดังนี้

```dart
// lib/pages/chat/chat_page.dart

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
      body:ChatInput(),
    );
  }
}
```

## 3. สร้าง UI และ Layout 

กลับมาที่ `lib/pages/chat/chat_input_component.dart` และเขียนโค้ดตามนี้

```dart
// lib/pages/chat/chat_input_component.dart

import 'package:flutter/material.dart';


class ChatInput extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(8.0),
      color: Colors.white,
      // ใช้ Row เพื่อจัดวาง TextField และ IconButton ในแนวนอน
      child: Row(
        children: [
          // TextField สำหรับพิมพ์ข้อความ
          TextField(
              // กำหนด decoration ให้กับ TextField เพื่อแสดง hintText
              decoration: InputDecoration(
                hintText: 'Type a message',
              ),
            ),
          // IconButton สำหรับส่งข้อความ
          IconButton(
            // กำหนด Icon เป็น Icon ของการส่งข้อความ
            icon: Icon(Icons.send),
            onPressed: () {
                // function เพื่อส่งข้อความไปยัง Chat Room
            },
          ),
        ],
      ),
    );
  }
}
```

## 4. จัดให้ TextField กินพื้นที่ที่เหลือ

เพื่อให้ TextField กินพื้นที่ที่เหลือใน Row ให้เพิ่ม `Expanded` ครอบ `TextField` ดังนี้

```dart
กลับมาที่ `lib/pages/chat/chat_input_component.dart` และเขียนโค้ดตามนี้

```dart
// lib/pages/chat/chat_input_component.dart

import 'package:flutter/material.dart';


class ChatInput extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(8.0),
      color: Colors.white,
      child: Row(
        children: [
            // ใช้ Expanded เพื่อให้ TextField กินพื้นที่ที่เหลือ
            Expanded(
              child: TextField(
                decoration: InputDecoration(
                  hintText: 'Type a message',
                ),
              ),
            ),
          IconButton(
            icon: Icon(Icons.send),
            onPressed: () {
            },
          ),
        ],
      ),
    );
  }
}
```

บันทึกไฟล์และทดสอบพรีวิวใน Emulator หรือ Device ของเรา

