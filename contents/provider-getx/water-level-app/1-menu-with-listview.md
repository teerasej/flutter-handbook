
# สร้างเมนูไปหน้าคำนวนด้วย ListView

```dart
// lib/pages/home_page/home_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Nextflow Water Gate')),

      // สร้างเมนูไปหน้าคำนวนด้วย ListView
      body: ListView(
        children: [
          ListTile(
            title: Text('คำนวน'),
            leading: Icon(Icons.calculate),
            onTap: () {
              Get.toNamed('/gate-calculator');
            },
          ),
        ],
      ),
    );
  }
}

```

