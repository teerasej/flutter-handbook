
# สร้างกลไกสำหรับเรียกใช้ Azure OpenAI Service ใน Controller

## 0. Resource

### URL
```
https://openai-nextflow.openai.azure.com/openai/deployments/gpt-4/chat/completions?api-version=2024-02-15-preview
```

### Key

[เอารหัสจากที่นี่ ](https://teerasej.notion.site/OpenAI-Service-key-by-Nextflow-19a57679357d8067b482d5f4f48674c4?pvs=73)


## 1. สร้างกลไกการทำงานแบบ Async 

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

  void handleSend() async {
    String inputValue = textController.text;
    if (inputValue.isNotEmpty) {
      messages.add(ChatMessage(message: inputValue, isSender: true));
      textController.clear();

      // ส่งข้อความไปยัง Azure OpenAI
      await sendMessageToAzureOpenAi(inputValue);
    }
  }

  // method นี้ทำงานแบบ async โดยจะส่งข้อความไปยัง Azure OpenAI
  Future<void> sendMessageToAzureOpenAi(String message) async {
    return;
  }
}
```

## 2. 

```dart
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
  
  // สร้าง Dio instance สำหรับใช้งานกับ Azure OpenAI
  final Dio _dio = Dio();
  // กำหนด URL และ API Key สำหรับใช้งานกับ Azure OpenAI
  final String _azureOpenAiEndpoint = '';
  final String _apiKey = '';

  void handleSend() async {
    String inputValue = textController.text;
    if (inputValue.isNotEmpty) {
      messages.add(ChatMessage(message: inputValue, isSender: true));
      textController.clear();
      await sendMessageToAzureOpenAi(inputValue);
    }
  }

  Future<void> sendMessageToAzureOpenAi(String message) async {
    try {

      // ส่งข้อความไปยัง Azure OpenAI แบบ POST method
      final response = await _dio.post(
        // กำหนด URL และ API Key สำหรับใช้งานกับ Azure OpenAI
        _azureOpenAiEndpoint,

        // กำหนด header โดยใช้ key 'api-key' และ value เป็น API Key ที่ได้จาก Azure OpenAI
        options: Options(
          headers: {
            'Content-Type': 'application/json',
            'api-key': _apiKey
          },
        ),

        // กำหนด body ของ request โดยใช้ข้อความที่ได้จากผู้ใช้
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

      // ถ้า response สำเร็จ และ status code เป็น 200
      if (response.statusCode == 200) {

        // ดึงข้อความที่ได้จาก response มาแสดงผล
        final responseMessage = response.data['choices'][0]['message']['content'];

        // เพิ่มข้อความที่ได้จาก response ไปยัง Chat History
        messages.add(ChatMessage(message: responseMessage, isSender: false));
      } else {

        // ถ้าการทำงานไม่สำเร็จ แสดงข้อความ Error และ status message ที่ได้จาก response
        messages.add(ChatMessage(
            message: 'Error: ${response.statusMessage}', isSender: false));
      }
    } catch (e) {
      // ถ้าเกิดข้อผิดพลาด แสดงข้อความ Error และข้อความที่เกิดข้อผิดพลาด
      messages.add(ChatMessage(message: 'Error: $e', isSender: false));
    }
  }
}

```

บันทึกไฟล์และรันโปรเจค จากนั้นลองส่งข้อความไปยัง Chat Input และกดปุ่ม Send ดู จะเห็นว่าข้อความที่ได้จาก Azure OpenAI จะถูกแสดงใน Chat History ของเรา