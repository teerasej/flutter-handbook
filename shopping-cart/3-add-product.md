
# เพิ่ม Product ลง Notifier 

เปิดไฟล์ **lib/components/product_list.dart**

```dart
// ทำการเรียนกใช้ provider package เพื่อให้ context มี method เพิ่มขึ้นจากการทำ extension method
import 'package:provider/provider.dart';


Widget build(BuildContext context) {
    return ListView.separated(
      itemCount: products.length,
      itemBuilder: (BuildContext context, int index) {
        var product = products[index];
        return ListTile(
          title: Text(product.name),
          subtitle: Text('${product.price} บาท'),
          onTap: () {
            // อ่าน instance ของ CartNotifier ออกมาจาก Context ผ่าน .read() และเรียกใช้ method ที่เตรียมไว้
            context.read<CartNotifier>().addProductToCart(product);
          },
        );
      },
      separatorBuilder: (BuildContext context, int index) {
        return Divider();
      },
    );
  }
```