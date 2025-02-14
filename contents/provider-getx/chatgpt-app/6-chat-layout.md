
# จัดการพื้นที่ของ Chat History และ Chat Input

## 1. กำหนดให้ ChatHistory กินพื้นที่เหลือในหน้าจอ

เราจะกำหนดให้ ChatHistory กินพื้นที่เหลือในหน้าจอ โดยใช้ Expanded widget ในการครอบ ChatHistory และ  ใน Chat Page

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
      body: Column(
        children: [
          // Expanded ใช้ในการกินพื้นที่เหลือในหน้าจอ
          Expanded(
            child: ChatHistory(),
          ),
          ChatInput(),
        ],
      ),
    );
  }
}
```

จะสังเกตว่า ChatHistory กินพื้นที่เหลือในหน้าจอ และ ChatInput ถูกดันลงมาอยู่ด้านล่างของหน้าจอ ซึ่งในบางอุปกรณ์อาจจะมีปัญหาเรื่องการแสดงผลของ Chat Input

## 2. ใช้งาน SafeArea ในการป้องกันการแสดงผลของ Chat Input

เราจะใช้ SafeArea widget ในการป้องกันการแสดงผลของ Chat Input ในบางอุปกรณ์ที่มีปัญหา

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
      body: Column(
        children: [
          Expanded(
            child: ChatHistory(),
          ),
          // SafeArea ใช้ในการป้องกันการแสดงผลของ Chat Input
          SafeArea(
            child: ChatInput(),
          ),
        ],
      ),
    );
  }
}

```