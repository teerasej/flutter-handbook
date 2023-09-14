
# สร้าง Data Repository สำหรับจัดการโค้ดส่วนติดต่อกับ Web API


เราจะสร้าง **Data Repository** ขึ้นมาเป็นเชื่อมระหว่าง Data Source และส่วนอื่นๆ ของ Application โดยสร้างเป็นไฟล์ `lib/data/repositories/news_repository.dart`

## 1. สร้าง abstract class เป็นต้นแบบของ Repo แบบต่างๆ 

โดยในที่นี้เราจะสร้าง Abstract class เป็นตัวต้นแบบของ Repo แบบต่างๆ 

```dart
import 'package:fast_news_app/data/repositories/news_repository_abstract.dart';

abstract class NewsRepositoryAbstract {

  // กำหนด ให้ทุก class ที่ implement abstract class นี้ไป ต้องเขียน method getNews() ที่ return เป็น Future<NewsModel>
  Future<NewsModel> getNews();
}
```

## 2. สร้าง News Repository ที่เป็น mock up data 

```dart
// lib/data/repositories/news_repository.dart
import 'package:fast_news_app/data/repositories/news_repository_abstract.dart';
import 'package:fast_news_app/domain/entities/news_model/news.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';

// สร้าง NewsRepository ที่ implement NewsRepositoryAbstract 
class NewsRepositoryMockUp implements NewsRepositoryAbstract {

  // implement getNews() ตามที่กำหนดใน Abstract class
  @override
  Future<NewsModel> getNews() async {

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
