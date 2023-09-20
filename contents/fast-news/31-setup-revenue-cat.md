
# Setup RevenueCat in Flutter App

## 1. ติดตั้ง package 

เราจะติดตั้ง package [purchases_flutter](https://pub.dev/packages/purchases_flutter) ซึ่งมี RevenueCat เป็นเจ้าของ

```bash
flutter pub add purchases_flutter
```

## 2. สร้าง Subscription Repository 

สร้างไฟล์ `lib/data/repositories/revenue_cat_repository.dart`

```dart
// lib/data/repositories/revenue_cat_repository.dart

import 'dart:async';
import 'dart:io';

import 'package:flutter/services.dart';
import 'package:purchases_flutter/purchases_flutter.dart';

class RevenueCatRepository {
  static const entitlementId = 'premium';
  static const appleApiKey = '';
  static const googleApiKey = '';


  RevenueCatRepository() {}

  // เริ่มการทำงานโดยโหลด key เข้าไป
  Future<void> init() async {
    await Purchases.setLogLevel(LogLevel.debug);

    late PurchasesConfiguration configuration;
    if (Platform.isIOS) {
      configuration = PurchasesConfiguration(appleApiKey);
    } else if (Platform.isAndroid) {
      configuration = PurchasesConfiguration(googleApiKey);
    }
    await Purchases.configure(configuration);
  }

  // ดึง package จาก offering ที่กำหนดใน dashboard ของ revenue cat มาแสดง
  Future<Offerings> getOfferings() async {
    try {
      Offerings offerings = await Purchases.getOfferings();
      return offerings;
    } on PlatformException catch (e) {
      print('error: ${e.message}');
      rethrow;
    }
  }

  // เรียกใช้ส่วน subscribe package ด้วย revenue cat
  Future<bool> subscribePackage(Package productPackage) async {
    CustomerInfo customerInfo = await Purchases.purchasePackage(productPackage);
    return customerInfo.entitlements.all[entitlementId]?.isActive ?? false;
  }

  // เช็คว่ามีการ subscribe หรือยังด้วย entitledment Id ที่สร้างไว้ใน Revenue Cat dashboard
  Future<bool> isPremiumUser() async {
    CustomerInfo customerInfo = await Purchases.getCustomerInfo();
    return customerInfo.entitlements.all[entitlementId]?.isActive ?? false;
  }
}

```

## 3. สร้าง Use case ที่ใช้ในการแสดงรายการสินค้า  

```dart
// lib/domain/usecases/get_offering_use_case.dart

import 'package:purchases_flutter/purchases_flutter.dart';
import 'package:fast_news_app/data/repositories/revenue_cat_repository.dart';

class GetOfferingsUseCase {
  final RevenueCatRepository _repository;

  GetOfferingsUseCase(this._repository);

  Future<Offerings> execute() async {
    return await _repository.getOfferings();
  }
}

```

## 4. สร้าง Use case ที่ใช้ในการสมัครสมาชิก 


```dart
// lib/domain/usecases/subscribe_package_use_case.dart

import 'package:purchases_flutter/purchases_flutter.dart';
import 'package:fast_news_app/data/repositories/revenue_cat_repository.dart';

class SubscribePackageUseCase {
  final RevenueCatRepository _repository;

  SubscribePackageUseCase(this._repository);

  Future<bool> execute(Package productPackage) async {
    return _repository.subscribePackage(productPackage);
  }
}

```

## 5. สร้าง Pay wall controller 

```dart
// lib/presentations/controllers/paywall_controller.dart

import 'package:get/get.dart';
import 'package:purchases_flutter/purchases_flutter.dart';
import 'package:fast_news_app/domain/usecases/get_offering_use_case.dart';
import 'package:fast_news_app/domain/usecases/subscribe_package_use_case.dart';

class PayWallController extends GetxController {
  final offering = Rxn<Offering>();

  // กำหนด Use case 2 ตัวสำหรับ controller นี้
  GetOfferingsUseCase _getOfferingsUseCase;
  SubscribePackageUseCase _subscribePackageUseCase;
  PayWallController(this._getOfferingsUseCase, this._subscribePackageUseCase);

  // โหลด offering มาแสดง
  Future<void> listOffering() async {
    var newOfferings = await _getOfferingsUseCase.execute();

    offering(newOfferings.current);
  }

  // ซื้อ subscription
  Future<void> purchasePackage(Package productPackage) async {
    await _subscribePackageUseCase.execute(productPackage);
  }
}

```

## 6. สร้าง Paywall Page 

```dart
// lib/presentations/screens/subscription/paywall_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:purchases_flutter/purchases_flutter.dart';
import 'package:fast_news_app/data/repositories/revenue_cat_repository.dart';
import 'package:fast_news_app/domain/usecases/get_offering_use_case.dart';
import 'package:fast_news_app/domain/usecases/subscribe_package_use_case.dart';
import 'package:fast_news_app/presentations/controllers/paywall_controller.dart';

class PayWallPage extends StatelessWidget {
  const PayWallPage({super.key});

  @override
  Widget build(BuildContext context) {
    final repository = Get.find<RevenueCatRepository>();

    final controller = Get.put(
      PayWallController(
        GetOfferingsUseCase(
          repository,
        ),
        SubscribePackageUseCase(
          repository,
        ),
      ),
    );

    controller.listOffering();

    return Scaffold(
      appBar: AppBar(
        title: Text('Subscription Plan'),
      ),
      body: Padding(
        padding: EdgeInsets.all(10),
        child: SizedBox(
            width: double.infinity,
            child: Obx(
              () {
                var offering = controller.offering.value;

                if (offering == null) {
                  return Center(
                    child: Text('No offering available.'),
                  );
                }

                return Column(
                  children: [
                    ListView.builder(
                      itemCount: offering.availablePackages.length,
                      itemBuilder: (BuildContext context, int index) {
                        var product = offering.availablePackages;

                        return ListTile(
                          title: Text(product[index].storeProduct.title),
                          subtitle:
                              Text(product[index].storeProduct.description),
                          trailing:
                              Text(product[index].storeProduct.priceString),
                          onTap: () {
                            controller.purchasePackage(product[index]);
                          },
                        );
                      },
                    ),
                    Spacer(),
                    OutlinedButton(
                      onPressed: () async {
                        await controller.restorePurchase();
                      },
                      child: Text('Restore purchase'),
                    ),
                  ],
                );
              },
            )),
      ),
    );
  }
}

```

## 7. สร้าง Revenue Cat Controller และเพิ่มหน้า Pay Wall เข้าไปในแอพ

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/firebase_repository.dart';
import 'package:fast_news_app/data/repositories/news_repository_web_api.dart';
import 'package:fast_news_app/data/repositories/revenue_cat_repository.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/domain/usecases/sign_out_user_use_case.dart';
import 'package:fast_news_app/presentations/controllers/app_link_controller.dart';

import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:fast_news_app/presentations/screens/authentication/sign_in_page.dart';
import 'package:fast_news_app/presentations/screens/authentication/sign_up_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_detail_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_page.dart';

import 'package:firebase_core/firebase_core.dart';
import 'package:fast_news_app/presentations/screens/subscription/paywall_page.dart';
import 'package:fast_news_app/presentations/screens/utillity/app_validate_page.dart';
import 'firebase_options.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    Get.put(GetConnect());

    Get.put<NewsController>(
      NewsController(
        GetNewsUseCase(
          NewsRepositoryWebAPI(),
        ),
        SignOutUserUseCase(
          FirebaseRepository(),
        ),
      ),
    );

    Get.put<AppLinkController>(
      AppLinkController(),
    );

    // สร้าง controller แบบ Async
    Get.putAsync<RevenueCatRepository>(() async {
      var repo = RevenueCatRepository();
      await repo.init();
      return repo;
    });

    return GetMaterialApp(
      title: 'Fast News',
      theme: ThemeData(
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
          useMaterial3: true,
          appBarTheme: AppBarTheme(
            color: Colors.blue[400],
            foregroundColor: Colors.white,
          )),
      initialRoute: '/',
      getPages: [
        // เพิ่มหน้า Revenue Cat
        GetPage(
          name: '/subscribe/start',
          page: () => const PayWallPage(),
        ),
        GetPage(
          name: '/news',
          page: () => const NewsPage(),
        ),
        GetPage(
          name: '/news/detail/:newsId',
          page: () => const NewsDetailPage(),
        ),
        GetPage(
          name: '/sign-in',
          page: () => const SignInPage(),
        ),
        GetPage(
          name: '/sign-up',
          page: () => const SignUpPage(),
        ),
        GetPage(
          name: '/',
          page: () => const AppValidatePage(),
        ),
      ],
    );
  }
}

```

## 8. ทำให้หน้า News Page เปิดไปหน้า Pay wall ได้ 

เริ่มจากที่ News Controller ก่อน

```dart
// lib/presentations/controllers/news_controller.dart

// presentation/controllers/post_controller.dart
import 'package:get/get.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/domain/usecases/sign_out_user_use_case.dart';

class NewsController extends GetxController {
  final GetNewsUseCase getNewsUsecase;
  final SignOutUserUseCase signOutUserUseCase;
  var newsModel = NewsModel().obs;
  var isLoading = true.obs;

  NewsController(this.getNewsUsecase, this.signOutUserUseCase);

  Future<void> loadNews() async {
    isLoading(true);
    final response = await getNewsUsecase.execute();
    newsModel(response);
    isLoading(false);
  }

  Future<void> signOut() async {
    await signOutUserUseCase.execute();
    Get.defaultDialog(
      title: 'Sign Out',
      middleText: 'Sign Out Success',
      barrierDismissible: false,
      onConfirm: () {
        Get.offAllNamed('/');
      },
    );
  }

  // เปิดหน้า subscribe
  Future<void> openSubscribePage() async {
    await Get.toNamed('/subscribe/start');
  }
}

```

จากนั้นปรับ NewsPage ตาม 

```dart
// lib/presentations/screens/news/news_page.dart

import 'package:flutter/material.dart';
import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:get/get.dart';

class NewsPage extends StatelessWidget {
  const NewsPage({super.key});

  @override
  Widget build(BuildContext context) {
    final newsController = Get.find<NewsController>();

    newsController.loadNews();

    return Scaffold(
      appBar: AppBar(
        title: const Text('News'),
        automaticallyImplyLeading: false,
        actions: [

          // สร้างปุ่มเปิดหน้า subscribe
          IconButton(
            onPressed: () {
              newsController.openSubscribePage();
            },
            icon: const Icon(Icons.star),
          ),

          
          IconButton(
            onPressed: () {
              newsController.signOut();
            },
            icon: const Icon(Icons.logout),
          ),
        ],
      ),
      body: Obx(
        () {
          if (newsController.isLoading.value) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          } else {
            return ListView.builder(
              itemCount: newsController.newsModel.value.news?.length ?? 0,
              itemBuilder: (context, index) {
                final article = newsController.newsModel.value.news?[index];
                return ListTile(
                  title: Text(article?.headline ?? '...'),
                  subtitle: Text(article?.content ?? '...'),
                  onTap: () {
                    Get.toNamed(
                      '/news/detail/${article?.id}',
                    );
                  },
                );
              },
            );
          }
        },
      ),
    );
  }
}

```