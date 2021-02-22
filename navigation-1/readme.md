
# Contact App with Navigation 

เริ่มต้นจากการสร้างโปรเจค `my_tab`

และเปลี่ยนไฟล์ `lib/main.dart` ด้วยโค้ดด้านล่าง

```dart
import 'package:flutter/material.dart';

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
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Container(),
    );
  }
}

```


1. [สร้างหน้าแอพ](1-create-page.md)
2. [กำหนด Tab Navigator](2-create-tab-navigator.md)
3. [กำหนดหน้าตาของ Contact Page และ Setting Page](3-define-ui-for-app-page.md)
4. [สร้าง Model class สำหรับเป็นตัวแทนข้อมูล](4-create-model-class.md)
5. [สร้าง ListView จากข้อมูล](5-generate-list.md)
6. [สร้างหน้ารายละเอียดผู้ติดต่อ](6-create-contact-detail-page.md)
7. [เปิดหน้า Contact Detail และส่งข้อมูลจาก Contact Page](7-pass-contact-object.md)
8. [แสดงรายละเอียดผู้ติดต่อ](8-show-detail-page.md)
9. [สร้าง Navigator สำหรับส่วนรายชื่อผู้ติดต่อ](9-create-nested-navigator.md)
10. [แสดงปุ่มโทรออก และส่งอีเมลล์](10)10-show-call-and-email-button.md