
# สร้าง Connection Class สำหรับการติดต่อ Web API

เพื่อการเรียกใช้จากจุดต่างๆ ของแอพ ในที่นี่เราจะสร้าง Class สำหรับกำหนด instance ที่ทำหน้าที่รับส่ง request และ response จาก Web API 

ในที่นี้เราเลือกใช้ [Dio](https://pub.dev/packages/dio) 

```dart
import 'package:dio/dio.dart';

class Connection {

// endpoint URL ในที่นี้เราจะกำหนดเป็น Web API ที่โค้ชพลสร้างเตรียมเอาไว้ 
// ตอนใช้งานจริงสามารถสลับเป็น endpoint ของ Web API เราเองได้
  static String endpoint = 'https://note-api.azurewebsites.net';


  static Dio getDio() {

    // กำหนด option ด้วย baseURL สำหรับเรียก url ต่างๆ ใน Web API
    var options = BaseOptions(
      baseUrl: Connection.endpoint,
      connectTimeout: 10000,
    );

    // สร้าง instance ของ Dio class ด้วย option ที่กำหนด
    var dioClient = Dio(options);
    return dioClient;
  }
}

```