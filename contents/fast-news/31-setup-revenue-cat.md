
# Setup RevenueCat in Flutter App

## 1. ติดตั้ง package 

เราจะติดตั้ง package [purchases_flutter](https://pub.dev/packages/purchases_flutter) ซึ่งมี RevenueCat เป็นเจ้าของ

```bash
flutter pub add purchases_flutter
```

## 2. สร้าง Subscription Repository 

สร้างไฟล์ ``

```dart
import 'dart:async';
import 'dart:io';

import 'package:flutter/services.dart';
import 'package:purchases_flutter/purchases_flutter.dart';
import 'package:test_getx_clean/data/repositories/store_config.dart' as store;

class SubscriptionRepository {
  static const entitlementId = 'premium';
  static const appleApiKey = '';
  static const googleApiKey = '';

  static const footerText =
      """A purchase will be applied to your account upon confirmation of the amount selected. Subscriptions will automatically
      renew unless canceled within 24 hours of the end of the current period. You can cancel any time using your account settings.
      Any unused portion of a free trial will be forfeited if you purchase a subscription.""";

  SubscriptionRepository() {
    if (Platform.isIOS) {
      store.StoreConfig(
        store: store.Store.appleStore,
        apiKey: appleApiKey,
      );
    } else if (Platform.isAndroid) {
      store.StoreConfig(
        store: store.Store.googlePlay,
        apiKey: googleApiKey,
      );
    }
  }

  Future<Offerings> getOfferings() async {
    try {
      Offerings offerings = await Purchases.getOfferings();
      return offerings;
    } on PlatformException catch (e) {
      print('error: ${e.message}');
      rethrow;
    }
  }

  Future<bool> subscribePackage(Package productPackage) async {
    CustomerInfo customerInfo = await Purchases.purchasePackage(productPackage);
    return customerInfo.entitlements.all[entitlementId]?.isActive ?? false;
  }
}

```