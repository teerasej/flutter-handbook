
# สร้าง Navigator สำหรับส่วน Catalog Screen และ Setting Screen

ในส่วนนี้เราจะสร้าง Navigator สำหรับส่วน Catalog Page โดยเราจะสร้าง `CatalogNavigator` ซึ่งจะเป็น Navigator ที่เป็นตัวกลางในการเปลี่ยนหน้าของ Catalog Page และ detail page

## 1. สร้าง `onGenerateCatalogRoute` ใน `NavigationController`

```dart
// lib/controllers/nav/navigation_controller.dart

import 'package:flutter/material.dart';
import 'package:flutter_nextflow_navigation_with_getx/pages/item_catalog/catalog_page.dart';
import 'package:flutter_nextflow_navigation_with_getx/pages/item_catalog/detail_page.dart';
import 'package:flutter_nextflow_navigation_with_getx/pages/setting/setting_page.dart';
import 'package:flutter_nextflow_navigation_with_getx/pages/setting/setting_silent_page.dart';
import 'package:get/get.dart';

class NavigationController extends GetxController {
  var currentIndex = 0.obs;

  void changePage(int index) {
    currentIndex.value = index;
  }

  // สร้าง method สำหรับจัดการสร้าง route ของส่วน Catalog 
  Route? onGenerateCatalogRoute(RouteSettings settings) {

    // นำ route ที่ได้มาจาก settings.name มาเปรียบเทียบกับชื่อ route ที่เราต้องการ เพื่อให้แสดงหน้าแอพที่เราต้องการได้
    switch (settings.name) {

      // หากเป็น route ของ Catalog Page ให้สร้างหน้า Catalog Page
      case '/catalog':
        return GetPageRoute(
          settings: settings,
          page: () => CatalogPage(),
        );
      
      // หากเป็น route ของ Detail Page ให้สร้างหน้า Detail Page
      case '/catalog/detail':
        return GetPageRoute(
          settings: settings,
          page: () => DetailPage(),
        );
      
      // หากไม่ตรงกับเงื่อนไขใดๆ ให้ return null
      default:
        return null;
    }
  }

}
```

## 2. สร้าง `onGenerateCatalogRoute` ใน `NavigationController`

จากนั้น เราจะสร้าง method `onGenerateSettingsRoute` ใน `NavigationController` เพื่อจัดการการเปลี่ยนหน้าของ Setting Page และ Setting Silent Page ในรูปแบบเดียวกันกับ `onGenerateCatalogRoute`

```dart
// lib/controllers/nav/navigation_controller.dart
import 'package:flutter/material.dart';
import 'package:flutter_nextflow_navigation_with_getx/pages/item_catalog/catalog_page.dart';
import 'package:flutter_nextflow_navigation_with_getx/pages/item_catalog/detail_page.dart';
import 'package:flutter_nextflow_navigation_with_getx/pages/setting/setting_page.dart';
import 'package:flutter_nextflow_navigation_with_getx/pages/setting/setting_silent_page.dart';
import 'package:get/get.dart';

class NavigationController extends GetxController {
  var currentIndex = 0.obs;

  void changePage(int index) {
    currentIndex.value = index;
  }

  Route? onGenerateCatalogRoute(RouteSettings settings) {
    switch (settings.name) {
      case '/catalog':
        return GetPageRoute(
          settings: settings,
          page: () => CatalogPage(),
        );
      case '/catalog/detail':
        return GetPageRoute(
          settings: settings,
          page: () => DetailPage(),
        );
      default:
        return null;
    }
  }

  // สร้าง method สำหรับจัดการสร้าง route ของส่วน Setting โดยในที่นี้จะนำไปใช้กับ Navigator ของ Setting Screen 
  Route? onGenerateSettingsRoute(RouteSettings settings) {

    // นำ route ที่ได้มาจาก settings.name มาเปรียบเทียบกับชื่อ route ที่เราต้องการ เพื่อให้แสดงหน้าแอพที่เราต้องการได้
    switch (settings.name) {

      // หากเป็น route ของ Setting Page ให้สร้างหน้า Setting Page
      case '/settings':
        return GetPageRoute(
          settings: settings,
          page: () => SettingPage(),
        );
    
      // หากเป็น route ของ Setting Silent Page ให้สร้างหน้า Setting Silent Page
      case '/settings/silent':
        return GetPageRoute(
          settings: settings,
          page: () => SettingSilentPage(),
        );
      default:
        return null;
    }
  }
}
```


## 3. สร้าง `Custom Navigator` สำหรับทั้ง 2 ส่วน ใน Main Screen

หลังจากนั้น เราจะนำ Navigator ที่เราสร้างไว้มาใช้งานใน Main Screen โดยใช้ `IndexedStack` และ `Navigator` ในการแสดงหน้าแอพ

```dart
// lib/main_screen.dart

import 'package:flutter/material.dart';
import 'package:flutter_nextflow_navigation_with_getx/controllers/nav/navigation_controller.dart';
import 'package:get/get.dart';
class MainScreen extends StatelessWidget {

  final NavigationController navController = Get.put(NavigationController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        
        // สังเกตว่าเรามีการใช้ Obx เพื่อให้การเปลี่ยนค่า current index มีผลกับการแสดงผลของ IndexStack
      body: Obx(() {

        // ใช้ IndexedStack เพื่อให้สามารถแสดงหน้าที่เป็น Navigator ได้หลายหน้า
        return IndexedStack(

          // กำหนด index ของ Navigator ที่ต้องการแสดง
          index: navController.currentIndex.value,
          children: [

            // สร้าง Navigator สำหรับ Catalog Page
            Navigator(
              
              // กำหนด key เพื่อให้สามารถเข้าถึง Navigator นี้ได้
              key: Get.nestedKey(1),

              // กำหนด initial route ของ Navigator นี้เป็น 
              initialRoute: '/catalog',

              // กำหนดการ generate route ของ Navigator นี้ จาก method ที่เตรียมไว้ใน controller
              onGenerateRoute: navController.onGenerateCatalogRoute,
            ),

            // ทำเหมือนกันกับ Navigator ของ Catalog Page แต่เป็นสำหรับ Setting Page
            Navigator(

              // สังเกตว่าเรากำหนด key ให้เป็น Get.nestedKey(2) เพื่อให้สามารถเข้าถึง Navigator นี้ได้ และแตกต่างจาก Navigator ก่อนหน้า ที่กำหนดเป็น Get.nestedKey(1)
              key: Get.nestedKey(2),

              // กำหนด initial route ของ Navigator นี้สำหรับ Setting Page
              initialRoute: '/settings',

              // กำหนดการ generate route ของ Navigator นี้ จาก method ที่เตรียมไว้ใน controller
              onGenerateRoute: navController.onGenerateSettingsRoute,
            ),
          ],
        );
      }),
      bottomNavigationBar: Obx(
        () => BottomNavigationBar(
          items: const [
            BottomNavigationBarItem(
              icon: Icon(Icons.list),
              label: 'Catalog',
            ),
            BottomNavigationBarItem(
              icon: Icon(Icons.settings),
              label: 'Settings',
            ),
          ],
          currentIndex: navController.currentIndex.value,
          onTap: navController.changePage,
        ),
      ),
    );
  }
}
```

หลังจากนั้น ลองรันโปรเจคของเรา จะเห็นว่าเราสามารถเปลี่ยนหน้าได้จากการกดที่ Bottom Navigator Bar และสามารถเปลี่ยนหน้าได้ในแต่ละ Navigator ได้