
# การสร้าง Dart Class ด้วย JsonToDart VSCode Extension

## 1. ติดตั้ง JSONtoDart Extension 

ทำการติดตั้ง Extension นี้ [JSON to Dart Model](https://marketplace.visualstudio.com/items?itemName=hirantha.json-to-dart)

## 2. คัดลอก JSON 

ตัวอย่าง JSON อยู่ใน https://nextflow-azure-function-node-news-api.azurewebsites.net/api/getNews

ให้ทำการ copy JSON นี้ไว้

## 3. สร้าง Model Class 

1. ใน Visual Studio Code ให้เปิด Command Palette (Ctrl + P, Cmd + P) 
2. เลือกคำสั่ง **JSON to Dart: From Clipboard** และทำตามขั้นตอน
   1. ตั้งชื่อ base class: **NewsModel**
   2. Implement Equality Operator: **No**
   3. You you want to use immutable class: **No**
   4. Implement `toString`: **No**
   5. Implement `copyWith`: **No**
   6. Suffix Json method: **Json**
   7. Implement Map JSON Method...: **No**
   8. สร้างโฟลเดอร์ `lib/domain/entities/` และเลือก directory นี้

จะได้ไฟล์มา 2 ไฟล์ 

### lib/domain/entities/news_model/news_model.dart

```dart
import 'package:get/get.dart';

import 'news.dart';

class NewsModel {
  List<News>? news;

  NewsModel({this.news});

  factory NewsModel.fromJson(Map<String, dynamic> json) => NewsModel(
        news: (json['news'] as List<dynamic>?)
            ?.map((e) => News.fromJson(e as Map<String, dynamic>))
            .toList(),
      );

  Map<String, dynamic> toJson() => {
        'news': news?.map((e) => e.toJson()).toList(),
      };
}

```
### lib/domain/entities/news_model/news.dart

```dart
class News {
  String? headline;
  String? content;
  String? link;

  News({this.headline, this.content, this.link});

  factory News.fromJson(Map<String, dynamic> json) => News(
        headline: json['headline'] as String?,
        content: json['content'] as String?,
        link: json['link'] as String?,
      );

  Map<String, dynamic> toJson() => {
        'headline': headline,
        'content': content,
        'link': link,
      };
}
```

