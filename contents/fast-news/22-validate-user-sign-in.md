
# สร้างกลไกเช็คการ sign in ของ user

มี 10 ขั้นตอน

## 1. สร้าง Use case 

สร้างไฟล์ `lib/domain/usecases/sign_in_state_validate_use_case.dart`

```dart
// lib/domain/usecases/sign_in_state_validate_use_case.dart

class SignInStateValidateUsecase {
  Future<bool> execute() {
    return Future.value(true);
  }
}
```

## 2. สร้าง Controller 

สร้างไฟล์ `lib/presentations/controllers/app_controller.dart`

```dart
// lib/presentations/controllers/app_controller.dart

import 'package:get/get.dart';
import 'package:fast_news_app/domain/usecases/sign_in_state_validate_use_case.dart';

class AppController extends GetxController {
  SignInStateValidateUsecase _signInStateValidateUsecase;

  AppController(this._signInStateValidateUsecase);

  Future<void> validateSignInState() async {
    _signInStateValidateUsecase.execute();
  }
}

```

## 3. สร้างหน้า Loading สวยๆ 

หน้านี่แหละที่จะเป็นตัวเช็ค User sign in state 

```dart
// lib/presentations/screens/utillity/app_validate_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/domain/usecases/sign_in_state_validate_use_case.dart';
import 'package:fast_news_app/presentations/controllers/app_controller.dart';

class AppValidatePage extends StatelessWidget {
  const AppValidatePage({super.key});

  @override
  Widget build(BuildContext context) {
    Get.put(
      AppController(
        SignInStateValidateUsecase(),
      ),
    );

    return Container(
      color: Colors.white,
      child: Center(
        child: CircularProgressIndicator(),
      ),
    );
  }
}

```

## 4. เพิ่ม page เข้าไปใน app

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/news_repository_web_api.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';

import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:fast_news_app/presentations/screens/authentication/sign_in_page.dart';
import 'package:fast_news_app/presentations/screens/authentication/sign_up_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_detail_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_page.dart';

import 'package:firebase_core/firebase_core.dart';
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
      ),
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

      // ปรับให้ route เริ่มต้น เป็น AppValidatePage
      initialRoute: '/',
      getPages: [
        GetPage(
          name: '/news',
          page: () => const NewsPage(),
        ),
        GetPage(
          name: '/news/detail',
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

        // เพิ่ม AppValidatePage
        GetPage(
          name: '/',
          page: () => const AppValidatePage(),
        ),
      ],
    );
  }
}

```

## 5. เรียกใช้ controller ใน Page เพื่อทำการเช็ค User Sign in state

```dart
// lib/presentations/screens/utillity/app_validate_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/domain/usecases/sign_in_state_validate_use_case.dart';
import 'package:fast_news_app/presentations/controllers/app_controller.dart';

class AppValidatePage extends StatelessWidget {
  const AppValidatePage({super.key});

  @override
  Widget build(BuildContext context) {

    // สร้าง AppController ที่มี SignInStateValidateUsecase
    final appController = Get.put(
      AppController(
        SignInStateValidateUsecase(),
      ),
    );

    // สั่งเริ่มเช็คสถานะของ User 
    appController.validateSignInState();

    return Container(
      color: Colors.white,
      child: Center(
        child: CircularProgressIndicator(),
      ),
    );
  }
}
```

## 6. เปิดไป Page ที่เหมาะสม ตามผลที่ได้จากการเรียกใช้ Use case

```dart
import 'package:get/get.dart';
import 'package:fast_news_app/domain/usecases/sign_in_state_validate_use_case.dart';

class AppController extends GetxController {
  SignInStateValidateUsecase _signInStateValidateUsecase;

  AppController(this._signInStateValidateUsecase);

  Future<void> validateSignInState() async {

    // ผลจากการเรียกใช้ Use case จะเป็น boolean 
    var isUserSignIn = await _signInStateValidateUsecase.execute();

    if (isUserSignIn) {
      // เปิดไปหน้าแสดงข่าว ถ้าพบว่าผู้ใช้ sign in ไว้อยู่
      Get.offAllNamed('/news');
    } else {
      // เปิดไปหน้า sign in ถ้าพบว่าผู้ใช้ไม่ได้ log in
      Get.offAllNamed('/sign-in');
    }
  }
}

```

## 7. เปิด `firebase_repository` เพื่อเพิ่มกลไกเช็คสถานะของ user sign in 

เปิดไฟล์ `lib/data/repositories/firebase_repository.dart` 

และเพิ่ม method เข้าไป

```dart
// lib/data/repositories/firebase_repository.dart

import 'package:firebase_auth/firebase_auth.dart';

class FirebaseRepository {
  Future<UserCredential> signInUser(String email, String password) async {
    return FirebaseAuth.instance.signInWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  Future<UserCredential> createUser(String email, String password) async {
    return FirebaseAuth.instance.createUserWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  // คืนค่าเป็นผลลัพธ์ของการเรียกดูข้อมูล currentUser
  // ถ้า User ไม่ได้ login ส่วนนี้จะเป็น null
  Future<bool> isUserSignIn() async {
    return FirebaseAuth.instance.currentUser != null;
  }
}

```

## 8. เรียกใช้ Repo ใน sign_in_state_validate_use_case.dart

```dart
// lib/domain/usecases/sign_in_state_validate_use_case.dart

import 'package:fast_news_app/data/repositories/firebase_repository.dart';

class SignInStateValidateUsecase {

  // กำหนดให้ Usecase สามารถเรียกใช้ Firebase Repo
  final FirebaseRepository _firebaseRepository;
  SignInStateValidateUsecase(this._firebaseRepository);

  Future<bool> execute() async {
    // เรียกใช้ Firebase Repo เพื่อดูว่า user ตอนนี้ sign in อยู่ไหม
    return _firebaseRepository.isUserSignIn();
  }
}

```

## 9. กำหนดใช้งาน Firebase repo ใน Use case 

```dart
// lib/presentations/screens/utillity/app_validate_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/firebase_repository.dart';
import 'package:fast_news_app/domain/usecases/sign_in_state_validate_use_case.dart';
import 'package:fast_news_app/presentations/controllers/app_controller.dart';

class AppValidatePage extends StatelessWidget {
  const AppValidatePage({super.key});

  @override
  Widget build(BuildContext context) {
    final appController = Get.put(
      AppController(
        SignInStateValidateUsecase(

          // สร้าง FirebaseRepository เพื่อให้ Use case ใช้งาน
          FirebaseRepository(),
        ),
      ),
    );

    appController.validateSignInState();

    return Container(
      color: Colors.white,
      child: Center(
        child: CircularProgressIndicator(),
      ),
    );
  }
}

```

## 10. ทดสอบการทำงาน