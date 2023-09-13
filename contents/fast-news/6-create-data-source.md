
# สร้าง data source สำหรับจัดการโค้ดส่วนติดต่อกับ Web API

เราจะสร้าง **Data Source** ขึ้นมาเป็นส่วนของโค้ดที่ติดต่อกับ Web API โดยตรง โดยสร้างเป็นไฟล์ `lib/data/datasources/news_data_source.dart`

```dart
// lib/data/datasources/news_data_source.dart

import 'package:fast_news_app/domain/entities/news_model/news.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';

class NewsDataSource {

  // สร้าง fetchNews method ที่ทำงานแบบ Asynchronous
  Future<NewsModel> fetchNews() async {
    
    // ส่วนนี้คือการเรียกใช้งาน Web API เว้นไว้ก่อน

    // สร้าง Mock up ของข้อมูล News Model ที่ได้มาจาก Web API
    return NewsModel(news: [
      News(
        headline: 'Headline 1',
        link: 'https://www.google.com',
        content: 'Content 1',
      ),
      News(
        headline: 'Headline 2',
        link: 'https://www.google.com',
        content: 'Content 2',
      ),
    ]);
  }
}

```