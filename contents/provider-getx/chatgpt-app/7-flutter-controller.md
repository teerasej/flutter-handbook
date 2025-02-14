
# การใช้งานระบบ Controller ของ Flutter

ในบทนี้เราจะมาเรียนรู้เกี่ยวกับการใช้งานระบบ Controller ใน Flutter ในการดึงและกำหนดข้อมูลให้กับ Widget ต่าง ๆ ในแอปพลิเคชันของเรา

## การใช้งาน Controller ใน Flutter

เปิดไฟล์ `lib/pages/chat/chat_input_component.dart` และเรียกใช้งาน `TextEditingController` ในการจัดการข้อมูลของ TextField ใน Chat Input ของเรา

```dart
// lib/pages/chat/chat_input_component.dart

import 'package:flutter/material.dart';

class ChatInput extends StatelessWidget {

  // สร้าง TextEditingController สำหรับจัดการข้อมูลของ TextField
  final TextEditingController _controller = TextEditingController();

  // ฟังก์ชันสำหรับดึงข้อมูลจาก TextField มาใช้งาน และทำการล้างข้อมูลใน TextField
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
              // กำหนด Controller ให้กับ TextField
              controller: _controller,
              decoration: InputDecoration(
                hintText: 'Type a message',
              ),
            ),
          ),
          IconButton(
            icon: Icon(Icons.send),
            // เรียกใช้งานฟังก์ชัน _handleSend เมื่อมีการกดปุ่ม Send
            onPressed: () => _handleSend(context),
          ),
        ],
      ),
    );
  }
}

```

บันทึกไฟล์และทดสอบการทำงานของ Chat Input ในแอปพลิเคชันของเรา