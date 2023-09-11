
# สร้าง Navigator สำหรับส่วนรายชื่อผู้ติดต่อ

จะเห็นว่าการใช้คำสั่ง push โดยตรงจะทำให้หน้า Contact Detail Page เปิดทับ Tab Menu 

เพราะกลายเป็นว่าคำสั่ง push ใช้ Navigator เดียวกันกับ Tab Navigator

เราสามารถแก้ไขได้ โดยสร้าง Navigator แยกสำหรับ Contact Page อีกที

```dart
// lib/pages/contact_page_builder.dart

import 'package:flutter/material.dart';
import 'package:nextflow_navigation_tab_stack/pages/contact_page.dart';

class ContactPageBuilder extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
      // คืนค่าเป็น Navigator object
    return Navigator(
        // กำหนดค่าเป็น PageRoute ตอนสร้าง Navigator
      onGenerateRoute: (RouteSettings settings) {
        return MaterialPageRoute(
            // คืนค่าเป็น Contact Page Widget
          builder: (context) => ContactPage(),
          settings: settings,
        );
      },
    );
  }
}

```

จากนั้นนำ Contact Page Builder ไปใช้แทน Contact Page ในไฟล์ main.dart

```dart
// lib/main.dart
...
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: 2,
      initialIndex: 0,
      child: Scaffold(
        backgroundColor: Colors.blue,
        body: TabBarView(
          children: [
            // ใช้ ContactPageBuilder แทน ContactPage
            ContactPageBuilder(),
            SettingPage(),
          ],
        ),
        bottomNavigationBar: TabBar(
          tabs: [
            Tab(
              text: 'รายชื่อ',
            ),
            Tab(
              text: 'ตั้งค่า',
            )
          ],
        ),
      ),
    );
  }
}
```