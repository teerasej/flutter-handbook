
# สร้าง Model Class สำหรับเป็นตัวแทนข้อมูลของข้อความใน Chat History

ในส่วนนี้เราจะมาสร้าง Model Class สำหรับเป็นตัวแทนข้อมูลของข้อความใน Chat History โดยเราจะสร้าง Class ชื่อว่า `ChatMessage` ซึ่งจะเก็บข้อมูลของข้อความที่ส่งและรับในแชท รวมถึงข้อมูลอื่นๆ ที่เกี่ยวข้องด้วย

## 1. สร้าง Class Model

```dart
// lib/models/chat_message.dart

class ChatMessage {
  final String message;
  final bool isSender;

  ChatMessage({required this.message, required this.isSender});
}
```

## 2. สร้างที่เก็บข้อมูลของ Chat Message ใน Chat Controller

```dart
// lib/controllers/chat_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/models/chat_message.dart';

class ChatController extends GetxController {
  final TextEditingController textController = TextEditingController();

  // สร้าง List ที่เก็บข้อมูลของ Chat Message โดยใช้ RxList จาก GetX
  var messages = <ChatMessage>[
    ChatMessage(message: "Hello, how can I help you?", isSender: false),
    ChatMessage(message: "I need some information about your services.", isSender: true)
  ];

  void handleSend() {
    String inputValue = textController.text;
    print(inputValue); 
    textController.clear(); 
  }

}
```