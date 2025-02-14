
# สร้าง GetX Controller สำหรับการจัดการข้อมูล และการทำงานของ Chat Input

ในส่วนนี้เราจะสร้าง GetX Controller สำหรับการจัดการข้อมูลและการทำงานของ Chat Input โดยเราจะสร้าง `ChatInputController` ซึ่งจะเป็นตัวควบคุมการทำงานของ `ChatInput` และ `ChatHistory` โดยเราจะสร้างฟังก์ชันสำหรับการส่งข้อความและการเรียกใช้งาน API ของ ChatGPT ด้วย

## 1. สร้าง GetX Controller

สร้างไฟล์ `lib/controllers/chat_controller.dart` และเพิ่มโค้ดดังนี้:

```dart
// lib/controllers/chat_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class ChatController extends GetxController {
  
}

```

## 2. นำเข้า ChatController ไปใช้งานใน ChatInput widget

เราจะนำ `ChatController` ไปใช้งานใน `ChatInput` widget โดยการใช้ `Get.put<ChatController>()` ซึ่งจะเป็นการสร้าง instance ของ `ChatController` ใส่ไว้ใน GetX และสามารถเรียกใช้งานได้จากทุกที่ในแอพพลิเคชั่น

```dart
// lib/pages/chat/chat_input_component.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/controllers/chat_controller.dart';

class ChatInput extends StatelessWidget {

  // สร้าง instance ของ ChatController ใส่ไว้ในระบบ GetX และดึงมาใช้งานในชื่อตัวแปร chatController
  final ChatController chatController = Get.put(ChatController());

  
  final TextEditingController _controller = TextEditingController();

  void _handleSend(BuildContext context) {
    String inputValue = _controller.text;
    print(inputValue); 
    _controller.clear(); 
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(8.0),
      color: Colors.white,
      child: Row(
        children: [
          Expanded(
            child: TextField(
              controller: _controller,
              decoration: InputDecoration(
                hintText: 'Type a message',
              ),
            ),
          ),
          IconButton(
            icon: Icon(Icons.send),
            onPressed: () => _handleSend(context),
          ),
        ],
      ),
    );
  }
}
```

บันทึกไฟล์และรีเฟรชหน้าจอของแอพพลิเคชั่น จะเห็นว่าเราสามารถใช้งาน `ChatController` ไม่มีปัญหาอะไร แต่จะสังเกต log ที่ยืนยันว่าเรามี chat controller ในระบบแล้ว

## 3. ย้าย TextEditingController ไปใส่ใน ChatController

เราจะย้าย `TextEditingController` ไปใส่ใน `ChatController` และสร้างฟังก์ชันสำหรับการส่งข้อความ

```dart
// lib/controllers/chat_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class ChatController extends GetxController {
  // ย้าย TextEditingController มาใส่ใน ChatController
  final TextEditingController textController = TextEditingController();
  
  // ย้ายฟังก์ชัน handleSend มาใส่ใน ChatController
  void handleSend() {
    String inputValue = textController.text;
    print(inputValue); 
    textController.clear(); 
  }
}
```

และทำการเปลี่ยนแปลงใน `ChatInput` widget ให้เรียกใช้งาน `ChatController` และเรียกใช้งาน `handleSend` จาก `ChatController`

```dart
// lib/pages/chat/chat_input_component.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/controllers/chat_controller.dart';

class ChatInput extends StatelessWidget {
  final ChatController chatController = Get.put(ChatController());

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(8.0),
      color: Colors.white,
      child: Row(
        children: [
          Expanded(
            child: TextField(
              // ใช้งาน textController จาก ChatController
              controller: chatController.textController,
              decoration: InputDecoration(
                hintText: 'Type a message',
              ),
            ),
          ),
          IconButton(
            icon: Icon(Icons.send),
            // เรียกใช้งาน handleSend จาก ChatController
            onPressed: () => chatController.handleSend(),
          ),
        ],
      ),
    );
  }
}

```

บันทึกไฟล์และรีเฟรชหน้าจอของแอพพลิเคชั่น จะเห็นว่าเราสามารถใช้งาน `ChatController` และ `handleSend` ได้เหมือนปกติ เพียงแต่เราย้าย `TextEditingController` ไปใส่ใน `ChatController` เพื่อให้สามารถเรียกใช้งานได้จากทุกที่ในแอพพลิเคชั่น และยังเป็นการจัดการ state ของข้อมูลในแอพพลิเคชั่นได้ดีขึ้น