
# สร้าง Notifier

Notifier เปรียบเหมือน Controller + Model ใน MVC มีจุดประสงค์เพื่อเป็นตัวกลางในการอัพเดต และแชร์ข้อมูลระหว่าง Widget 

1. สร้างไฟล์ **lib/controllers/cart_notifier.dart**
2. เขียนไฟล์ตามด้านล่างนี้ 

```dart
import 'dart:collection';

import 'package:flutter/material.dart';
import 'package:nextflow_shopping_provider_demo/models/product_in_cart_model.dart';
import 'package:nextflow_shopping_provider_demo/models/product_model.dart';

class CartNotifier extends ChangeNotifier {

  // สร้างตัวข้อมูลแบบ private เป็นตัวแทนของสินค้าที่หยิบมาไว้ในตะกร้า
  List<ProductModel> _shoppingCart = [];

  // Getter method ที่กำหนดให้เป็น ListView ที่แก้ไขข้อมูลไม่ได้ มีประโยชน์ในป้องกันการแก้ไขข้อมูลจากนอก notifier โดยตรง
  UnmodifiableListView<ProductModel> get shoppingCart =>
      UnmodifiableListView(_shoppingCart);

  // เพิ่มข้อมูลสินค้าละตะกร้า และแจ้งให้ watch ทุกตัวทราบในการดึงข้อมูลไปใช้งาน
  addProductToCart(ProductModel product) {
    _shoppingCart.add(product);

    // แจ้งให้ watch ทุกตัวทราบในการดึงข้อมูลไปใช้งาน
    notifyListeners();
  }

  // ส่งข้อมูลแบบที่คำนวนรวมจำนวนสินค้าแยกชนิดไว้
  List<ProductInCartModel> get checkoutSummary {
    List<ProductInCartModel> result = [];

    for (var product in _shoppingCart) {
      if (result.length >= 0) {
        var productExistInSummary = false;

        for (var productTotal in result) {
          if (productTotal.product.name == product.name) {
            productTotal.count += 1;
            productExistInSummary = true;
            break;
          }
        }

        if (!productExistInSummary) {
          result.add(ProductInCartModel(product));
        }
      }
    }

    return result;
  }

  // รวมราคาสินค้าทั้งหมด
  double get total {
    double result = 0;

    for (var product in _shoppingCart) {
      result += product.price as double;
    }

    return result;
  }
}


```