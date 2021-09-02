
# แสดงจำนวนสินค้าบนปุ่มตะกร้า 


เปิดไฟล์ **lib/components/checkout_button.dart**

```dart
// ทำการเรียนกใช้ provider package เพื่อให้ context มี method เพิ่มขึ้นจากการทำ extension method
import 'package:provider/provider.dart';


@override
  Widget build(BuildContext context) {
    return Badge(
      position: BadgePosition.topEnd(top: 5, end: 5),
      badgeContent: Text(
          // เรียกใช้ CartNotifier จาก context ผ่าน method .watch โดยส่วนนี้จะทำการดึงค่าที่กำหนดล่าสุดออกมา ทุกครั้งที่มีการเรียกใช้ changeNotifier() จาก provider
        '${context.watch<CartNotifier>().shoppingCart.length}',
        style: TextStyle(color: Colors.white, fontSize: 12),
      ),
      child: IconButton(
        icon: Icon(Icons.shopping_cart_outlined),
        onPressed: () {},
      ),
    );
  }
```