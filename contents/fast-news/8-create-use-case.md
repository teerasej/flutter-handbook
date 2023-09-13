
# สร้าง Use case ที่เป็น action ที่สั่งโหลดข้อมูล

เราจะสร้าง **Use case** ขึ้นมาเป็นตัวแทนของ use case ที่เป็นการเรียกใช้  โดยสร้างเป็นไฟล์ `lib/domain/usecases/get_news_use_case.dart`
`

```dart
// lib/domain/usecases/get_news_use_case.dart

import 'package:fast_news_app/data/repositories/news_repository.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';

class GetNewsUseCase {

  final NewsRepository repository;

  // กำหนด News Repository
  GetNewsUseCase(this.repository);

  // สร้าง execute method สำหรับเริ่มดำเนินการ action ที่ต้องการ
  Future<NewsModel> execute() {
    return repository.getNews();
  }
}

```