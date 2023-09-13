
# สร้าง News Controller 

เราจะสร้าง **Controller** ขึ้นมาเป็นจุดที่โต้ตอบระหว่าง Widget และส่วนอื่นๆ ของ Application โดยสร้างเป็นไฟล์ `lib/presentations/controllers/news_controller.dart`

ในที่นี้เราเรียกใช้ GetxController ในการทำงานด้วย

```dart
// lib/presentations/controllers/news_controller.dart

import 'package:get/get.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';

// ทำการ extends GetxController เพื่อให้สามารถทำงานเข้ากับ Get.put ได้
class NewsController extends GetxController {

  // เก็บใช้งาน use case
  final GetNewsUseCase getNewsUsecase;

  // สร้าง news model เป็นตัวแปร observable
  var newsModel = NewsModel().obs;

  // สร้าง isLoading เป็นตัวแปร observable
  var isLoading = true.obs;

  
  NewsController(this.getNewsUsecase);

  // สร้าง method ที่ดำเนินการ set ค่าตัวแปรที่มีการดึงไปใช้งาน ตาม flow
  Future<void> loadNews() async {

    // กำหนด isLoading เป็น true เพื่อแสดงตัวโหลดดิ้งใน Obx ของหน้า News Page
    isLoading(true);

    // เรียก use case เพื่อดึงข้อมูล news มาจาก 
    final response = await getNewsUsecase.execute();

    // นำข้อมูลที่ได้มาอัพเดตค่าในตัวแปร observable
    newsModel(response);

    // กำหนด isLoading เป็น false เพื่อซ่อนตัวโหลดดิ้งใน Obx ของหน้า News Page และแสดง ListView ที่ดึงข้อมูล News model ไปแทน
    isLoading(false);
  }
}

```