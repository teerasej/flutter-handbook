
# สร้างตัวบอกสถานะการตอบกลับของ OpenAI Service

## 1. สร้างตัวแปร Rx สำหรับเก็บสถานะการทำงานของ OpenAI Service

```dart
// lib/controllers/chat_controller.dart

import 'package:dio/dio.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/models/chat_message.dart';

class ChatController extends GetxController {
  final TextEditingController textController = TextEditingController();
  var messages = <ChatMessage>[
    ChatMessage(message: "Hello, how can I help you?", isSender: false),
    ChatMessage(
        message: "I need some information about your services.", isSender: true)
  ].obs;

  // สร้างตัวแปร Rx สำหรับเก็บสถานะการทำงานของ OpenAI Service
  var isTyping = false.obs; 


  final Dio _dio = Dio();
  final String _azureOpenAiEndpoint = '';
  final String _apiKey = '';

  void handleSend() async {
    String inputValue = textController.text;
    if (inputValue.isNotEmpty) {
      messages.add(ChatMessage(message: inputValue, isSender: true));
      textController.clear();

      // กำหนดค่าให้ isTyping เป็น true ก่อนที่จะส่งข้อความไปยัง Azure OpenAI
      isTyping.value = true;
      await sendMessageToAzureOpenAi(inputValue);

       // กำหนดค่าให้ isTyping เป็น false เมื่อได้รับข้อความตอบกลับจาก Azure OpenAI
      isTyping.value = false;
    }
  }

  Future<void> sendMessageToAzureOpenAi(String message) async {
    try {
      final response = await _dio.post(
        _azureOpenAiEndpoint,
        options: Options(
          headers: {
            'Content-Type': 'application/json',
            'api-key': _apiKey
          },
        ),
        data: {
          "messages": [
            {
              "role": "system",
              "content":
                  "You are an AI assistant that helps people find information."
            },
            {
              "role": "user",
              "content": message,
            },
          ],
          "temperature": 0.7,
          "max_tokens": 800,
        },
      );

      if (response.statusCode == 200) {
        final responseMessage = response.data['choices'][0]['message']['content'];
        messages.add(ChatMessage(message: responseMessage, isSender: false));
      } else {
        messages.add(ChatMessage(
            message: 'Error: ${response.statusMessage}', isSender: false));
      }
    } catch (e) {
      messages.add(ChatMessage(message: 'Error: $e', isSender: false));
    }
  }
}

```

## 2. แสดงสถานะการทำงานของ OpenAI Service ใน UI

```dart
// lib/pages/chat/chat_history_component.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/controllers/chat_controller.dart';
import 'package:nextflow_chatgpt/pages/chat/chat_message_balloon_component.dart';

class ChatHistory extends StatelessWidget {
   ChatHistory({super.key});

   final ChatController chatController = Get.find();

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.grey[200],
      child: Obx(() {
        return Column(
          children: [
            Expanded(
              child: ListView.builder(
                itemCount: chatController.messages.length,
                itemBuilder: (context, index) {
                  final message = chatController.messages[index];
                  return ChatMessageBalloon(
                    message: message.message,
                    isSender: message.isSender,
                  );
                },
              ),
            ),

            // แสดงสถานะการทำงานของ OpenAI Service ตามเงื่อนไขของ isTyping จาก ChatController
            if (chatController.isTyping.value)
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Align(
                  alignment: Alignment.centerLeft,
                  child: Text(
                    'Thinking...',
                    style: TextStyle(
                      fontStyle: FontStyle.italic,
                      color: Colors.grey,
                    ),
                  ),
                ),
              ),
          ],
        );
      }),
    );
  }
}

```

บันทึกและรันโปรเจค และลองส่งข้อความไปยัง ChatGPT ดูว่าสถานะการทำงานของ OpenAI Service จะแสดงอย่างไรใน UI หรือไม่