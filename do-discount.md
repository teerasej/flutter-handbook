
# Do Discount App

## สิ่งที่ต้องมีก่อนเริ่มทำ

- อย่างน้อยต้องมีการสร้างโปรเจคใหม่ และมีไฟล์ `lib/main.dart` 

## เริ่มทำโปรเจค 

เปิดไฟล์ `lib/main.dart`  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Nextflow Do Discount',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Container()
    );
  }
}
```

### 1. สร้าง Scaffold

เขียน และกำหนด Scaffold Widget ในส่วน `home:` ของ `MaterialApp()` 

```dart
home: Scaffold(
        appBar: AppBar(
          title: Text('Do Discount'),
        ),
        body: Container(
          padding: EdgeInsets.all(20.0),
          child: Column(
            children: <Widget>[
              Form(
                  child: Column(
                children: <Widget>[
				  Text('ราคา'),
                  TextFormField(),
				  Text('ส่วนลด'),
                  TextFormField(),
                  RaisedButton(
                    onPressed: () {
                      print('do something');
                    },
                    child: Text('Calculate'),
                  )
                ],
              ))
            ],
          ),
        ),
      ),
```

#### เสร็จแล้วไฟล์ `lib/main.dart` จะได้แบบนี้

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Nextflow Do Discount',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        appBar: AppBar(
          title: Text('Do Discount'),
        ),
        body: Container(
          padding: EdgeInsets.all(20.0),
          child: Column(
            children: <Widget>[
              Form(
                  child: Column(
                children: <Widget>[
				  Text('ราคา'),
                  TextFormField(),
				  Text('ส่วนลด'),
                  TextFormField(),
                  RaisedButton(
                    onPressed: () {
                      print('do something');
                    },
                    child: Text('Calculate'),
                  )
                ],
              ))
            ],
          ),
        ),
      ),
    );
  }
}
```

### 2. กำหนดค่า keybordType เป็น number

Widget `TextFormField` สามารถกำหนดรูปแบบของ keyboard ที่แสดงได้ด้วย `keyboardType` 

ในที่นี้ เราจะกำหนดให้ช่องข้อความ แสดงแค่แป้นพิมพ์ตัวเลข

``` dart
TextFormField(
     keyboardType: TextInputType.number,
),
```

### 3. สร้างส่วน Stateful Widget

เราจะสร้าง Class ใหม่ขึ้นมาโดยให้ extend class `Stateful` 

ซึ่งถ้าเราติดตั้งส่วนเสริมของ Visual Studio Code แล้ว เราสามารถใช้คำสั่งด้านล่างเพื่อสร้างโค้ดสำเร็จ จากรายการเมนูได้

![2020-03-31_19-00-06](https://user-images.githubusercontent.com/85179/78024349-66735a80-7382-11ea-82e2-a2854655d46d.png)


โดยย้ายส่วน `Container` มาใส่ไว้ใน `Stateful` Widget

```dart
class MainPage extends StatefulWidget {
  @override
  _MainPageState createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return Container(
          padding: EdgeInsets.all(20.0),
          child: Column(
            children: <Widget>[
              Form(
                  child: Column(
                children: <Widget>[
                  Text('ราคา'),
                  TextFormField(
                    keyboardType: TextInputType.number,
                  ),
                  Text('ส่วนลด'),
                  TextFormField(
                    keyboardType: TextInputType.number,
                  ),
                  RaisedButton(
                    onPressed: () {
                      print('do something');
                    },
                    child: Text('Calculate'),
                  )
                ],
              ))
            ],
          ),
        );
  }
}
```

และนำส่วน `MainPage` widget ที่เราสร้างขึ้นมา กำหนดลงไปในค่า `home:` แทน Container ตัวเก่า

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Nextflow Do Discount',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        appBar: AppBar(
          title: Text('Do Discount'),
        ),
        // ตรงนี้ล่ะ ที่เราเอา MainPage widget มาใช้
        body: MainPage()
      ),
    );
  }
}
```

### 4. ดึงข้อความจาก TextField

`TextFormField` จะมีส่วนที่เรียกว่า `controller:` ที่สามารถจับคู่กับ object ของ class `TextEditingController` ได้

เราเลยจะสร้าง 2 property ใน class `_MainPageState` 

```dart
class _MainPageState extends State<MainPage> {
  
  TextEditingController priceTextCtrl = new TextEditingController();
  TextEditingController discountTextCtrl = new TextEditingController();
  ...
}
```

และนำมากำหนดให้กับ `TextFormField` ก็ถือว่าใช้ได้

```dart
TextFormField(
   keyboardType: TextInputType.number,
   controller: priceTextCtrl,
),
...
TextFormField(
   keyboardType: TextInputType.number,
   controller: discountTextCtrl,
),
```

### 5. กำหนดกลไกการทำงานของ Widget ด้วย Event

จะสังเกตว่าใน `RaisedButton()` widget จะมี attribute ชื่อ `onPressed` ซึ่งรับค่าเป็น function

ค่าพวกนี้เรียกว่า event ก็ได้ครับ

```dart
RaisedButton(
    onPressed: () {
        print('do something');
    },
    child: Text('Calculate'),
)
```

### 6. เรียกใช้ข้อมูลจากช่องกรอก

จากนั้นเราก็สามารถเรียกใช้ `TextEditingController` ที่ต้องการเพื่อเรียกใช้ใน function

```dart
RaisedButton(
    onPressed: () {
        print('Price: ${priceTextCtrl.text}, discount: ${discountTextCtrl.text}');

        var price = double.parse(priceTextCtrl.text);
        var discount = double.parse(discountTextCtrl.text);

        var result = price * (price - discount)/100;

        print('Result is ${result}');
	},
    child: Text('Calculate'),
)
```

### 7. แสดง Result บนหน้าแอพ

เราจะทำการเพิ่มข้อความที่ไว้แสดงผลลัพธ์ไว้ในหน้าแอพ 

```dart
Expanded(
	child: Center(
		child: Text('$_result', style: TextStyle(fontSize: 50.0))
	)
)
```

และทำให้ปุ่มมีขนาดกว้างเต็มความกว้างของจอด้วยการครอบ `SizedBox()` ให้กับ `RaisedButton()`

```dart
SizedBox(
   	width: double.infinity,
   	child: RaisedButton(
		...
	)
),
```

### 8. ทดสอบรันแอพพลิเคชั่น

### 9. ไฟล์สำเร็จของ `lib/main.dart`