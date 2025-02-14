
# นำ Obx มาใช้งานกับ Chat History

ในส่วนนี้เราจะมาใช้งาน `Obx` ในการแสดงข้อมูลของ Chat History ที่เราสร้างไว้ใน `ChatController` ที่เราสร้างไว้ใน [บทที่ 9](9-chat-input-controller.md) โดยเราจะใช้ `Obx` ในการแสดงข้อมูลของ `messages` ที่เราสร้างไว้ใน `ChatController` และแสดงใน `ChatHistory` Widget ที่เราสร้างไว้ใน [บทที่ 4](4-chat-history.md)

## 1. การกำหนด RxList ให้กับ messages ใน ChatController

```dart
// lib/controllers/chat_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/models/chat_message.dart';

class ChatController extends GetxController {
  final TextEditingController textController = TextEditingController();

  // สร้าง List ที่เก็บข้อมูลของ Chat Message โดยใช้ type RxList จาก GetX ผ่านการเรียกใช้ `.obs`
  var messages = <ChatMessage>[
    ChatMessage(message: "Hello, how can I help you?", isSender: false),
    ChatMessage(message: "I need some information about your services.", isSender: true)
  ].obs;

  void handleSend() {
    String inputValue = textController.text;

    // เพิ่มข้อความใหม่เข้าไปใน messages list ถ้า inputValue ไม่ว่าง
    if (inputValue.isNotEmpty) {
      // เพิ่มข้อความใหม่เข้าไปใน messages list โดยใช้ `.add()` ของ RxList
      messages.add(ChatMessage(message: inputValue, isSender: true));
      textController.clear();
    }
  }

}

```

## 2. นำ Chat controller มาใช้งานใน ChatHistory Widget

```dart
// lib/pages/chat/chat_history_component.dart

import 'package:flutter/material.dart';
import 'package:nextflow_chatgpt/pages/chat/chat_message_balloon_component.dart';

class ChatHistory extends StatelessWidget {
  const ChatHistory({super.key});

  // ดึง ChatController จากระบบ GetX มาใช้งาน
   final ChatController chatController = Get.find();

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
บันทึกและ save ไฟล์ ตอนนี้จะเห็นว่ามี Error เกิดขึ้นประมาณว่าหา ChatController ไม่เจอ เพราะว่าในระบบ GetX อาจจะยังไม่มี instance ของ ChatController อยู่ (ถึงแม้ว่าเราจะมีการใช้ `Get.put()` ใน ChatInput ก็ตาม แต่ลำดับการสร้างอาจจะไม่ทัน)


## 3. ย้ายการสร้าง ChatController ไปที่ ChatPage

เราจะทำการสร้าง ChatController ใน ChatPage และใช้งานใน ChatHistory แทน

```dart
// lib/pages/chat/chat_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/controllers/chat_controller.dart';

import 'chat_history_component.dart';
import 'chat_input_component.dart';

class ChatPage extends StatelessWidget {
   ChatPage({super.key});

  // สร้าง ChatController ใน ChatPage แทนที่จะเป็นใน ChatHistory หรือ ChatInput
  final ChatController chatController = Get.put(ChatController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Chat Page'),
      ),
      body: Column(
        children: [
          Expanded(
            child: ChatHistory(),
          ),
          SafeArea(
            child: ChatInput(),
          ),
        ],
      ),
    );
  }
}
```

จากนั้นก็เปลี่ยนให้ ChatInput เรียกใช้งาน ChatController แทนการสร้างใส่เข้าไปในระบบ

```dart
// lib/pages/chat/chat_input_component.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/controllers/chat_controller.dart';

class ChatInput extends StatelessWidget {

  // ดึง ChatController จากระบบ GetX มาใช้งาน แทนที่จะสร้างและใส่เพิ่มในนี้
  final ChatController chatController = Get.find();


  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(8.0),
      color: Colors.white,
      child: Row(
        children: [
          Expanded(
            child: TextField(
              controller: chatController.textController,
              decoration: InputDecoration(
                hintText: 'Type a message',
              ),
            ),
          ),
          IconButton(
            icon: Icon(Icons.send),
            onPressed: () => chatController.handleSend(),
          ),
        ],
      ),
    );
  }
}
```

บันทึกและ save ไฟล์ และรีเฟรชหน้าจอ จะเห็นว่าปัญหา Error ที่หา ChatControler ไม่เจอ ได้หายไปแล้ว


## 4. นำ Obx มาใช้งานใน ChatHistory Widget


1. Widget `Obx` ของ GetX จะทำการ rebuild หน้าจอทุกครั้งที่ข้อมูลในตัวแปรกลุ่ม `Rx` มีการเปลี่ยนแปลง เช่นในที่นี้คือ `messages` ใน `ChatController` 
2. หากใช้งาน `Obx` widget แต่ไม่มีการเรียกใช้งานตัวแปร `Rx` ใดๆ ภายใน function จะเกิด error ขึ้น

```dart
// lib/pages/chat/chat_history_component.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/controllers/chat_controller.dart';
import 'package:nextflow_chatgpt/pages/chat/chat_message_balloon_component.dart';

class ChatHistory extends StatelessWidget {
   ChatHistory({super.key});

   // ดึง ChatController จากระบบ GetX มาใช้งาน
   final ChatController chatController = Get.find();

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.grey[200],

      // ใช้ Obx ในการแสดงข้อมูลของ messages list ที่เราสร้างไว้ใน ChatController
      child: Obx(() {

        // ใช้ ListView.builder ในการแสดงข้อมูลของ messages list ที่เราสร้างไว้ใน ChatController
        return ListView.builder(

          // กำหนดจำนวนของ items ให้เท่ากับจำนวนของ messages list ที่เราสร้างไว้ใน ChatController
          itemCount: chatController.messages.length,

          // สร้าง ChatMessageBalloon สำหรับแสดงข้อความในแชท
          itemBuilder: (context, index) {

            // ดึงข้อมูลของ message จาก messages list ที่เราสร้างไว้ใน ChatController
            final message = chatController.messages[index];

            // ส่งข้อมูลของ message ไปยัง ChatMessageBalloon สำหรับแสดงข้อความในแชท
            return ChatMessageBalloon(
              message: message.message,
              isSender: message.isSender,
            );
          },
        );
      }),
    );
  }
}
```

บันทึกและ save ไฟล์ และรีเฟรชหน้าจอ จะเห็นว่าเมื่อเราส่งข้อความเข้าไปใน ChatInput แล้วกดปุ่ม Send ข้อความใหม่จะถูกเพิ่มเข้าไปใน ChatHistory อย่างทันที