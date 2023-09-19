
# สร้างส่วนของการจ่ายเงินโดยใช้ Omise Key

## 1. สร้าง Omise Repository 

```dart
// lib/data/repositories/omise_repository.dart

import 'dart:convert';

import 'package:get/get.dart';
import 'package:test_getx_clean/domain/entities/charge_omise_request.dart';
import 'package:test_getx_clean/domain/entities/omise_charge_response/omise_charge_response.dart';
import 'package:test_getx_clean/domain/entities/omise_token_response/omise_token_response.dart';

class OmiseRepository {
  final connect = Get.find<GetConnect>();

  // นำ key ที่ได้มาใช้
  final publicKey = "";
  final secretKey = "";

  Future<bool> charge(ChargeOmiseRequest request) async {

    // สร้างข้อมูลบัตรเพื่อส่งไปให้ Omise สร้าง token ของบัตร
    final chargeOmiseRequestData = {
      'card': {
        'name': request.name,
        'city': 'Bangkok',
        'postal_code': '10150',
        'number': request.cardNumber,
        'expiration_month': request.expirationMonth.toString(),
        'expiration_year': request.expirationYear.toString(),
        'security_code': request.securityCode
      }
    };

    // เรียกใช้ Web API
    var requestTokenResponse = await connect.post(
      'https://vault.omise.co/tokens',
      chargeOmiseRequestData,
      headers: {
        'Authorization': 'Basic ${base64Encode(utf8.encode(publicKey))}',
      },
    );

    if (requestTokenResponse.statusCode == 200) {
      var omiseTokenReponse =
          OmiseTokenResponse.fromMap(requestTokenResponse.body);

      // นำ token ของบัตรที่ได้จาก token response มาใช้งาน
      var token = omiseTokenReponse.id;

      // สร้างข้อมูลของการชาร์จ
      final chargeData = {
        'amount': request.amount,
        'currency': 'thb',
        'card': token,
      };

      // ส่ง charge data ไปที่ web api
      var chargeWithTokenResponse = await connect.post(
        'https://api.omise.co/charges',
        chargeData,
        headers: {
          'Authorization': 'Basic ${base64Encode(utf8.encode(secretKey))}',
        },
      );

      if (chargeWithTokenResponse.statusCode == 200) {
        var chargeResponse =
            OmiseChargeResponse.fromMap(chargeWithTokenResponse.body);

        // นำค่า paid ของ charge response มาตัดสิน
        return chargeResponse.paid ?? false;
      }
    }

    return false;
  }
}

```

## 2. สร้าง Use case 

```dart
// lib/domain/usecases/charge_omise_use_case.dart

import 'package:test_getx_clean/data/repositories/omise_repository.dart';
import 'package:test_getx_clean/domain/entities/charge_omise_request.dart';

class ChargeOmiseUseCase {
  final OmiseRepository omiseRepository;

  ChargeOmiseUseCase(this.omiseRepository);

  Future<bool> execute(ChargeOmiseRequest request) async {
    return await omiseRepository.charge(request);
  }
}

```

## 3. สร้าง Controller 

```dart 


import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:test_getx_clean/domain/entities/charge_omise_request.dart';
import 'package:test_getx_clean/domain/usecases/charge_omise_use_case.dart';

class OmiseController extends GetxController {
  final formKeyCharge = GlobalKey<FormState>();

  // สร้าง field เก็บข้อมูลจาก form
  String name = '';
  String cardNumber = '';
  int expirationMonth = 0;
  int expirationYear = 0;
  String securityCode = '';

  ChargeOmiseUseCase _chargeOmiseUseCase;

  OmiseController(this._chargeOmiseUseCase);

  void tryCharge() async {
    final isValid = formKeyCharge.currentState!.validate();
    if (true) {
      formKeyCharge.currentState!.save();
      print(name);
      print(cardNumber);
      print(expirationMonth);
      print(expirationYear);
      print(securityCode);

      // สร้างข้อมูล card และระบุจำนวนเงิน
      final request = ChargeOmiseRequest(
        name: name,
        cardNumber: cardNumber,
        expirationMonth: expirationMonth,
        expirationYear: expirationYear,
        securityCode: securityCode,
        amount: 100000,
      );

      // ข้อมูลจำลองการส่ง
      // final request = ChargeOmiseRequest(
      //   name: 'pon',
      //   cardNumber: '4242424242424242',
      //   expirationMonth: 10,
      //   expirationYear: 2025,
      //   securityCode: '234',
      //   amount: 100000,
      // );

      
      try {
        var result = await _chargeOmiseUseCase.execute(request);

        // แสดง popup ผลการทำระเงิน
        if (result) {
          Get.defaultDialog(
            title: "Success",
            content: Text("Charge success."),
          );
        } else {
          Get.defaultDialog(
            title: "Oops",
            content: Text("Unable to charge."),
          );
        }
      } catch (e) {
        Get.defaultDialog(
          title: "Oops",
          content: Text("Unable to charge. ${e.toString()}"),
        );
      }
    }
  }

  String? isEmptyValidator(String value) {
    if (value.isEmpty) {
      return 'Please enter a valid value.';
    }
    return null;
  }

  String? isValidCreditCardNumber(String value) {
    if (value.isEmpty || value.length < 16) {
      return 'Please enter a valid value.';
    }
    return null;
  }

  String? isValidExpirationMonth(String value) {
    if (value.isEmpty || int.parse(value) > 12) {
      return 'Please enter a valid value.';
    }
    return null;
  }

  String? isValidExpirationYear(String value) {
    if (value.isEmpty || value.length < 4) {
      return 'Please enter a valid value.';
    }
    return null;
  }

  String? isValidSecurityCode(String value) {
    if (value.isEmpty || value.length < 3) {
      return 'Please enter a valid value.';
    }
    return null;
  }
}

```

## 4. สร้าง Omise Page

```dart
// lib/presentations/screens/purchase/omise_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:test_getx_clean/data/repositories/omise_repository.dart';
import 'package:test_getx_clean/domain/usecases/charge_omise_use_case.dart';
import 'package:test_getx_clean/presentations/controllers/omise_controller.dart';

class OmisePage extends StatelessWidget {
  const OmisePage({super.key});

  @override
  Widget build(BuildContext context) {
    final controller = Get.put(
      OmiseController(
        ChargeOmiseUseCase(
          OmiseRepository(),
        ),
      ),
    );

    return Scaffold(
        appBar: AppBar(
          title: Text('Purchase'),
        ),
        // the body contain a form with following field

        body: Padding(
          padding: EdgeInsets.all(8),
          child: Form(
            key: controller.formKeyCharge,
            child: Column(
              children: [
                // Email field
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'Name',
                  ),
                  keyboardType: TextInputType.text,
                  validator: (value) {
                    return controller.isEmptyValidator(value!);
                  },
                  onSaved: (value) {
                    controller.name = value!;
                  },
                ),
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'Card Number',
                  ),
                  keyboardType: TextInputType.text,
                  validator: (value) {
                    return controller.isValidCreditCardNumber(value!);
                  },
                  onSaved: (value) {
                    controller.cardNumber = value!;
                  },
                ),
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'Expiration Month',
                  ),
                  keyboardType: TextInputType.number,
                  validator: (value) {
                    return controller.isValidExpirationMonth(value!);
                  },
                  onSaved: (value) {
                    controller.expirationMonth = int.parse(value!);
                  },
                ),
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'Expiration Year',
                  ),
                  keyboardType: TextInputType.number,
                  validator: (value) {
                    return controller.isValidExpirationYear(value!);
                  },
                  onSaved: (value) {
                    controller.expirationYear = int.parse(value!);
                  },
                ),
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'Security code',
                  ),
                  keyboardType: TextInputType.number,
                  validator: (value) {
                    return controller.isValidSecurityCode(value!);
                  },
                  onSaved: (value) {
                    controller.securityCode = value!;
                  },
                ),
                Spacer(),
                SizedBox(
                  width: double.maxFinite,
                  child: ElevatedButton(
                    child: const Text('Pay 1000 THB'),
                    onPressed: () {
                      controller.tryCharge();
                    },
                  ),
                ),
              ],
            ),
          ),
        ));
  }
}

```
## 5. เพ่ิม Omise Page เข้าไปใน App 

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:test_getx_clean/data/repositories/firebase_repository.dart';
import 'package:test_getx_clean/data/repositories/news_repository_web_api.dart';
import 'package:test_getx_clean/domain/usecases/get_news_use_case.dart';
import 'package:test_getx_clean/domain/usecases/sign_out_user_use_case.dart';
import 'package:test_getx_clean/presentations/controllers/app_link_controller.dart';

import 'package:test_getx_clean/presentations/controllers/news_controller.dart';
import 'package:test_getx_clean/presentations/screens/authentication/sign_in_page.dart';
import 'package:test_getx_clean/presentations/screens/authentication/sign_up_page.dart';
import 'package:test_getx_clean/presentations/screens/news/news_detail_page.dart';
import 'package:test_getx_clean/presentations/screens/news/news_page.dart';

import 'package:firebase_core/firebase_core.dart';
import 'package:test_getx_clean/presentations/screens/purchase/omise_page.dart';
import 'package:test_getx_clean/presentations/screens/utillity/app_validate_page.dart';
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

    return GetMaterialApp(
      title: 'Fast News',
      theme: ThemeData(
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
          useMaterial3: true,
          appBarTheme: AppBarTheme(
            color: Colors.blue[400],
            foregroundColor: Colors.white,
          )),

       // กำหนดให้หน้าแรกเป็นหน้าจ่ายเงิน   
      initialRoute: '/purchase/omise',
      getPages: [
        GetPage(
          name: '/purchase/omise',
          page: () => const OmisePage(),
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


## 6. ปรับให้หน้า News Page เปิดมาที่หน้าจ่ายเงินได้ 

เพิ่ม method ใน controller ของ NewsController ก่อน

```dart
// presentation/controllers/news_controller.dart
import 'package:get/get.dart';
import 'package:test_getx_clean/domain/entities/news_model/news_model.dart';
import 'package:test_getx_clean/domain/usecases/get_news_use_case.dart';
import 'package:test_getx_clean/domain/usecases/sign_out_user_use_case.dart';

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

  // เปิดหน้าจ่ายเงินด้วย omise
  Future<void> openPurchasePage() async {
    Get.toNamed('/purchase/omise');
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
}

```

จากนั้นปรับให้ News Page มีปุ่มเปิดไปจ่ายเงิน โดยเรียกใช้งาน controller 

```dart
// lib/presentations/screens/news/news_page.dart

import 'package:flutter/material.dart';
import 'package:test_getx_clean/presentations/controllers/news_controller.dart';
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

          // เพิ่มปุ่ม
          IconButton(
            onPressed: () {
              newsController.openPurchasePage();
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