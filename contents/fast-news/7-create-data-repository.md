
# สร้าง Data Repository สำหรับเป็นตัวแทนของ Data ไว้เรียกใช้ใน Application



เราจะสร้าง **Data Repository** ขึ้นมาเป็นเชื่อมระหว่าง Data Source และส่วนอื่นๆ ของ Application โดยสร้างเป็นไฟล์ `lib/data/repositories/news_repository.dart`
`

```dart
// lib/data/repositories/news_repository.dart
import 'package:fast_news_app/data/datasources/news_data_source.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';

// สร้าง Abstract Class สำหรับใช้เป็น Template 
abstract class NewsRepositoryAbstract {
  
  // กำหนดให้ NewsRepositoryAbstract มี method ที่ต้อง implement ใช้งาน 1 ตัว
  Future<NewsModel> getNews();
}

// สร้าง NewsRepository ที่ implement NewsRepositoryAbstract โดยใน Repository ก็จะมีการเรียกใช้ Data source 
class NewsRepository implements NewsRepositoryAbstract {
  final NewsDataSource _dataSource;

  NewsRepository(this._dataSource);

  // implement getNews() ตามที่กำหนดใน Abstract class
  Future<NewsModel> getNews() {

    // เรียกใช้ data source ก็จะได้ Future object return ออกไป
    return _dataSource.fetchNews();
  }
}

```