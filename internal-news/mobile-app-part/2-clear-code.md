
# ล้างข้อมูลออกทั้งหมด

## 1. ล้างโค้ด template

เปิดไฟล์ และแก้ไขโค้ดให้เหลือแค่นี้ 

```dart
// lib/main.dart
import 'package:flutter/material.dart';

void main() {
  print('hello');
}
```

ทดลองบันทึกไฟล์ และดูผลลัพธ์

- น่าจะเห็นข้อความเดียวกับที่ใส่ให้ function `print()` แสดงขึ้นมาในส่วน **debug console** ของ Visual Studio Code
- หน้าแอพจะไม่แสดงอะไรขึ้นมา เป็นหน้าโล่งๆ

<img width="1408" alt="2021-08-15_15-36-36" src="https://user-images.githubusercontent.com/85179/129472501-67c80732-1be2-44f5-949e-52446e68f003.png">
