
# สร้างหน้าคำนวน และใส่ลงในแอพ

1. สร้างไฟล์ `lib/pages/gate_calculator_page/gate_calculator_page.dart`

```dart
// lib/pages/gate_calculator_page/gate_calculator_page.dart

import 'package:flutter/material.dart';

class GateCalculatorPage extends StatelessWidget {
  const GateCalculatorPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('คำนวนประตูน้ำ'),
      ),
    );
  }
}
```

2. และเพิ่มเมนูไปหน้าคำนวนในไฟล์ `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_flutter_water_gate_2/pages/gate_calculator_page/gate_calculator_page.dart';
import 'package:nextflow_flutter_water_gate_2/pages/home_page/home_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      title: 'Nextflow Water Gate',
      theme: ThemeData(
        primaryColor: Colors.blue[900],
      ),

      // กำหนดให้เริ่มต้นที่หน้าคำนวน
      initialRoute: '/gate-calculator',
      
      getPages: [
        GetPage(
          name: '/',
          page: () => HomePage(),
        ),

        // เพิ่มเมนูไปหน้าคำนวน
        GetPage(
          name: '/gate-calculator',
          page: () => GateCalculatorPage(),
        ),
      ],
    );
  }
}

```

