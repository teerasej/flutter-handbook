
# แสดงช่องกรอกข้อมูล

## 1. แสดงช่องกรอกข้อมูลแรก

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

        // แสดงช่องกรอกข้อมูล
      body: TextField(
        decoration: InputDecoration(
          labelText: 'จำลองระดับน้ำเหนือเขื่อน (m MSL)',
        ),
        keyboardType: TextInputType.number,
      ),
    );
  }
}
```

## 2. ครอบด้วย column widget เพื่อใส่ช่องกรอกข้อมูลเพิ่ม

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
      // ครอบด้วย column widget เพื่อใส่ช่องกรอกข้อมูลเพิ่ม
      body: Column(
        children: [
          TextField(
            decoration: InputDecoration(
              labelText: 'จำลองระดับน้ำเหนือเขื่อน (m MSL)',
            ),
            keyboardType: TextInputType.number,
          ),
        ],
      ),
    );
  }
}

```

## 3. ใส่ช่องกรอกข้อมูลที่เหลือ และใส่ padding

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
      body: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Column(
          children: [
            TextField(
              decoration: InputDecoration(
                labelText: 'จำลองระดับน้ำเหนือเขื่อน (m MSL)',
              ),
              keyboardType: TextInputType.number,
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'ระดับน้ำที่ต้องการยกระดับ (m MSL)',
              ),
              keyboardType: TextInputType.number,
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'แผนระบายน้ำ (m MSL)',
              ),
              keyboardType: TextInputType.number,
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'Total Sum SNR Release (MCM)',
              ),
              keyboardType: TextInputType.number,
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'Total Sum TTN Release (MCM)',
              ),
              keyboardType: TextInputType.number,
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'ปั้มสูบกลับ (m MSL)',
              ),
              keyboardType: TextInputType.number,
            ),
          ],
        ),
      ),
    );
  }
}


```