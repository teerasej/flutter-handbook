
# กำหนด Tab Navigator

เราจะแก้ไขหน้าตาของ Widget MyApp จาก

```dart
// lib/main.dart

...

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Container(),
    );
  }
}
```

เป็น


```dart
// lib/main.dart

import 'package:nextflow_navigation_tab_stack/pages/contact_page.dart';
import 'package:nextflow_navigation_tab_stack/pages/setting_page.dart';


class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
      // DefaultTabController จะเป็นส่วนที่ควบคุมการแสดงผลของ Tab Navigator
    return DefaultTabController(
    // กำหนดจำนวน Tab
      length: 2,
    // กำหนด Tab index เริ่มต้น
      initialIndex: 0,

    // กำหนด Widget ที่แสดงใน DefaultTabController
      child: Scaffold(
        backgroundColor: Colors.blue,
        // TabBarView ไว้ใช้แสดง Widget สำหรับแต่ละ tab ที่เลือก
        body: TabBarView(
          children: [
            ContactPage(),
            SettingPage(),
          ],
        ),
        // TabBar ไว้แสดง Tab Menu
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

## ไฟล์เต็ม lib/main.dart

```dart
import 'package:flutter/material.dart';
import 'package:nextflow_navigation_tab_stack/pages/contact_page.dart';
import 'package:nextflow_navigation_tab_stack/pages/setting_page.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Nextflow Flutter Nav Tab Stack',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: MyHomePage(title: 'Contact'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

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
            ContactPage(),
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