
# ใส่ Slider

## 1. เว้นระยะจากช่องกรอก และใส่ slider

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

            // เว้นระยะจากช่องกรอก
            SizedBox(
              height: 16,
            ),

            // ใส่ slider
            Text('Unit 1'),
            Slider(
              value: 80,
              min: 80,
              max: 120,
              divisions: 4,
              onChanged: (double value) {
                print(value);
              },
            ),
          ],
        ),
      ),
    );
  }
}


```

## 2. ใส่ slider ที่เหลือ

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
            SizedBox(
              height: 16,
            ),
            Text('Unit 1 (80)'),
            Slider(
              value: 80,
              min: 80,
              max: 120,
              divisions: 4,
              onChanged: (double value) {
                print(value);
              },
            ),
            Text('Unit 2 (80)'),
            Slider(
              value: 80,
              min: 80,
              max: 120,
              divisions: 4,
              onChanged: (double value) {
                print(value);
              },
            ),
            Text('Unit 3 (80)'),
            Slider(
              value: 80,
              min: 80,
              max: 120,
              divisions: 4,
              onChanged: (double value) {
                print(value);
              },
            ),
            Text('Unit 4 (150)'),
            Slider(
              value: 150,
              min: 150,
              max: 180,
              divisions: 3,
              onChanged: (double value) {
                print(value);
              },
            ),
            Text('Unit 5 (150)'),
            Slider(
              value: 150,
              min: 150,
              max: 180,
              divisions: 3,
              onChanged: (double value) {
                print(value);
              },
            ),
          ],
        ),
      ),
    );
  }
}


```
## 3. เว้นระยะจาก slider และใส่ปุ่ม

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
            SizedBox(
              height: 16,
            ),
            Text('Unit 1 (80)'),
            Slider(
              value: 80,
              min: 80,
              max: 120,
              divisions: 4,
              onChanged: (double value) {
                print(value);
              },
            ),
            Text('Unit 2 (80)'),
            Slider(
              value: 80,
              min: 80,
              max: 120,
              divisions: 4,
              onChanged: (double value) {
                print(value);
              },
            ),
            Text('Unit 3 (80)'),
            Slider(
              value: 80,
              min: 80,
              max: 120,
              divisions: 4,
              onChanged: (double value) {
                print(value);
              },
            ),
            Text('Unit 4 (150)'),
            Slider(
              value: 150,
              min: 150,
              max: 180,
              divisions: 3,
              onChanged: (double value) {
                print(value);
              },
            ),
            Text('Unit 5 (150)'),
            Slider(
              value: 150,
              min: 150,
              max: 180,
              divisions: 3,
              onChanged: (double value) {
                print(value);
              },
            ),

            // เว้นระยะจาก slider
            SizedBox(
              height: 16,
            ),
            
            // ใส่ปุ่ม
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                onPressed: () {},
                child: const Text('คำนวน'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}


```