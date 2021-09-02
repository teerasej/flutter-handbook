
# ครอบแอพด้วย Provider 

เปิดไฟล์ **lib/main.dart**

เราจะทำการครอบ Provider ลงไปในระดับทั้งแอพ นั่นก็คือครอบ `MyApp()`

```dart
void main() {
  runApp(
    // เราสามารถกำหนด Provider ได้หลายตัวพร้อมกัน โดยใช้ MultiProvider
    MultiProvider(
      // กำหนด Provider ที่ต้องการลงไปใน List 
      providers: [
        // กำหนดการทำงานของ ChangeNotifierProvider โดย return เป็น instance ของ CartNotifier nี่ทำเตรียมไว้
        ChangeNotifierProvider(
          create: (context) {
            return CartNotifier();
          },
        )
      ],
      // กำหนด Widget ที่จะมีผลต่อ Provider ในทีนี่คือ MyApp widget และ widget ย่อยทั้งหมด
      child: MyApp(),
    ),
  );
}
```

## ไฟล์ lib/main.dart ที่ทำเสร็จแล้ว 

```dart
import 'package:flutter/material.dart';
import 'package:nextflow_shopping_provider_demo/controllers/cart_notifier.dart';
import 'package:nextflow_shopping_provider_demo/home_page.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(
          create: (context) {
            return CartNotifier();
          },
        )
      ],
      child: MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Nextflow Shopping',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.orange,
      ),
      home: HomePage(),
    );
  }
}

```