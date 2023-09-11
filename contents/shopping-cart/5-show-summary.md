
# แสดงข้อมูล, จำนวนสินค้า, และราคารวม

เปิดไฟล์ **lib/check_out_page.dart**

```dart
// ทำการเรียนกใช้ provider package เพื่อให้ context มี method เพิ่มขึ้นจากการทำ extension method
import 'package:provider/provider.dart';


@override
  Widget build(BuildContext context) {

    // ดึงข้อมูลสรุปสินค้าในตะกร้ามาเตรียมใช้งานกับ Widget 
    List<ProductInCartModel> productsInCart =
        context.read<CartNotifier>().checkoutSummary;

     // ดึงข้อมูลรวมราคาสินค้ามาเตรียมใช้งานกับ Widget 
    double total = context.read<CartNotifier>().total;

    return Scaffold(
      appBar: AppBar(
        title: Text('Checkout'),
      ),
      body: ListView.builder(
        itemCount: productsInCart.length,
        itemBuilder: (BuildContext context, int index) {
          var productInCart = productsInCart[index];

          return ListTile(
            title: Text(productInCart.product.name),
            subtitle: Text('${productInCart.product.price} บาท'),
            trailing: Text('x ${productInCart.count}'),
          );
        },
      ),
      bottomNavigationBar: BottomAppBar(
        child: Padding(
          padding: const EdgeInsets.all(8.0),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.end,
            children: [
              Text(
                  // แสดงราคารวมที่ต้องจ่าย
                'รวม $total บาท',
                style: TextStyle(fontSize: 45),
              ),
            ],
          ),
        ),
      ),
    );
  }
```