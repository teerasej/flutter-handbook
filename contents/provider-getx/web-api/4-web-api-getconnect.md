
# ส่ง request ไปขอข้อมูลจาก Web API ด้วย GetConnect

## 1. สร้างตัวแปรเก็บ List ของ ProfileModel 

```dart
// lib/controllers/profile_controller.dart

import 'package:get/get.dart';
import 'package:nextflow_flutter_getx_profiles_api/models/profile_model/profile_model.dart';

class ProfileController extends GetxController {
  
  // สร้าง List ของ ProfileModel
  var profileList = <ProfileModel>[];


  var loading = true.obs;

  loadDataFromWebAPI() async {
    loading(true);

    // ดึง GetConnect ที่มีการสร้างไว้ มาจาก GetX 
    var connect = Get.find<GetConnect>();

    // ส่ง GET Request ไปที่ URL เพื่อได้ข้อมูล JSON กลับมา
    var response = await connect.get(
      'https://651d740c44e393af2d59d2b4.mockapi.io/api/profiles',
    );

    // แปลง JSON ที่ได้มาเป็น Model ของเรา โดยใช้ ProfileModel.fromMap ที่เราสร้างไว้
    profileList = List<ProfileModel>.from(
      response.body.map(
        (json) => ProfileModel.fromMap(json),
      ),
    );

    // กำหนด loading เป็น false ส่วนนี้จะทำให้ Obx widget เกิดการ rebuild ตัวเองใหม่
    loading(false);
  }
}


```

## 2. สร้างเงื่อนไขในการแสดง ListView บนหน้าจอ จากตัวแปร obs ของ controller

```dart
// lib/pages/home_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_flutter_getx_profiles_api/controllers/profile_controller.dart';

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    var controller = Get.put(ProfileController());
    controller.loadDataFromWebAPI();

    return Scaffold(
      appBar: AppBar(
        title: Text('Profiles'),
      ),
      body: Obx(
        () {
          if (controller.loading.value) {
            return Center(
              child: CircularProgressIndicator(),
            );
          }

          // ถ้าเกิดค่า loading ของ controller เป็น false ถือว่าเราเอา controller.profileList มาใช้งานได้ 
          return ListView.builder(

            // ใช้จำนวน item ใน controller.profileList กำหนดจำนวน widget ที่ต้องสร้าง
            itemCount: controller.profileList.length,
            itemBuilder: (context, index) {

              // ใช้เลข index ในการดึง item ออกมาจาก controller.profileList
              var profile = controller.profileList[index];

              // สร้าง และ return ListTile widget
              return ListTile(
                leading: CircleAvatar(
                  backgroundImage: NetworkImage(profile.avatar ?? ''),
                ),
                title: Text(profile.name ?? ''),
                subtitle: Text(profile.phone ?? ''),
              );
            },
          );
        },
      ),
    );
  }
}


```
