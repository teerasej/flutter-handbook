
# สร้าง Chat Balloon สำหรับแสดงข้อความใน Chat History

ในส่วนนี้เราจะสร้าง Chat Balloon สำหรับแสดงข้อความใน Chat History ซึ่งจะมีการแยกสีพื้นหลังของข้อความตามผู้ส่งข้อความ

## 1. สร้าง Chat Message Balloon Widget

สร้าง `lib/pages/chat/chat_message_balloon_component.dart` และเขียนโค้ดตามนี้

```dart
// lib/pages/chat/chat_message_balloon_component.dart

import 'package:flutter/material.dart';

class ChatMessageBalloon extends StatelessWidget {
  final String message;
  final bool isSender;

  const ChatMessageBalloon({
    Key? key,
    required this.message,
    required this.isSender,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {

    var displayMessage = '';

    if (isSender) {
      displayMessage = 'You: $message';
    } else {
      displayMessage = 'Bot: $message';
    }    

    return Text(displayMessage);
  }
}
```

## 2. นำ ChatMessageBalloon ไปใช้ใน ChatHistory ด้วยข้อมูลจำลอง

แก้ไข `lib/pages/chat/chat_history_component.dart` ให้เป็นดังนี้

```dart
// lib/pages/chat/chat_history_component.dart

import 'package:flutter/material.dart';
import 'package:nextflow_chatgpt/pages/chat/chat_message_balloon_component.dart';

class ChatHistory extends StatelessWidget {
  const ChatHistory({super.key});

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.grey[200],
      child: ListView(
        children: [
          // ข้อความจาก User
          ChatMessageBalloon(
            message: "Hello, how are you?",
            isSender: true,
          ),
          // ข้อความจาก Bot
          ChatMessageBalloon(
            message: "I'm good, thank you!",
            isSender: false,
          ),
        ],
      ),
    );
  }
}
```

## 3. จัดหน้าตาของ ChatMessageBalloon ให้สวยงาม

กลับมาที่ `lib/pages/chat/chat_message_balloon_component.dart` และเขียนโค้ดตามนี้

```dart
import 'package:flutter/material.dart';

class ChatMessageBalloon extends StatelessWidget {
  final String message;
  final bool isSender;

  const ChatMessageBalloon({
    Key? key,
    required this.message,
    required this.isSender,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    
    // กำหนดตำแหน่งของ balloon และสีพื้นหลังของข้อความตามประเภทผู้ส่งข้อความ
    Alignment alignment;
    Color? color;

    if (isSender) {
      alignment = Alignment.centerRight;
      // สีพื้นหลังของข้อความจาก User
      color = Colors.blue[200];
    } else {
      alignment = Alignment.centerLeft;
      // สีพื้นหลังของข้อความจาก Bot
      color = Colors.green[200];
    }

    // สร้าง Balloon แสดงข้อความ
    return Align(
      alignment: alignment,
      child: Container(
        margin: EdgeInsets.symmetric(vertical: 5, horizontal: 10),
        padding: EdgeInsets.all(10),
        decoration: BoxDecoration(
          color: color,
          borderRadius: BorderRadius.circular(10),
        ),
        child: Text(
          message,
          style: TextStyle(color: Colors.black),
        ),
      ),
    );
  }

}
```

## 4. ใช้งาน Ternary Operator ในการกำหนดค่าที่ใช้งานใน ChatMessageBalloon

แก้ไข `lib/pages/chat/chat_message_balloon_component.dart` ให้เป็นดังนี้

```dart
import 'package:flutter/material.dart';

class ChatMessageBalloon extends StatelessWidget {
  final String message;
  final bool isSender;

  const ChatMessageBalloon({
    Key? key,
    required this.message,
    required this.isSender,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Align(
      
      // กำหนดตำแหน่งของ balloon และสีพื้นหลังของข้อความตามประเภทผู้ส่งข้อความ
      alignment: isSender ? Alignment.centerRight : Alignment.centerLeft,
      child: Container(
        margin: EdgeInsets.symmetric(vertical: 5, horizontal: 10),
        padding: EdgeInsets.all(10),
        decoration: BoxDecoration(

          // สีพื้นหลังของข้อความตามประเภทผู้ส่งข้อความ
          color: isSender ? Colors.blue[200] : Colors.green[200],
          borderRadius: BorderRadius.circular(10),
        ),
        child: Text(
          message,
          style: TextStyle(color: Colors.black),
        ),
      ),
    );
  }

}
```



บันทึกและรีเฟรชหน้าจอของ Emulator หรือ Device จะได้ Chat Balloon ที่สวยงาม