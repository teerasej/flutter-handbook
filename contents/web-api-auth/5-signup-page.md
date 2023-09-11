
# สร้างหน้าลงทะเบียน

ในที่นี้หน้า Login จะมีปุ่มเปิดมาหน้าลงทะเบียน ซึ่งเราจะเริ่มจากจุดนี้

## 1. ดึงข้อมูลจาก Form มาสร้างเป็น Object

```dart
// lib/pages/signup_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"email": _email, "password": _password};

    }
}
```

## 2. ส่ง POST Request ไปที่เว็บ API เพื่อลงทะเบียน

```dart
// lib/pages/signup_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"email": _email, "password": _password};
        
        // เรียกใช้ Dio.post() เพื่อส่ง post request และรับ response กลับจาก web api
        final response = await Connection.getDio().post(
            // url สำหรับลงทะเบียน
            '/signup',
            // ใช้ json.encode() เพื่อแปลงข้อมูลให้อยู่ในรูปแบบ JSON format
            data: json.encode(data),
        );

    }
}
```

## 3. เช็คค่าที่ได้รับกลับจาก Web API และจัดการหน้าแอพ

```dart
// lib/pages/signup_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"email": _email, "password": _password};

        final response = await Connection.getDio().post(
            '/signup',
            data: json.encode(data),
        );

        // check ข้อมูลจาก Web API
        // ในที่นี้ถ้า statueCode เป็น 200 และค่า ['user']['ok'] = 1 ถือว่าลงทะเบียนผ่าน
        if (response.statusCode == 200 &&
        response.data['user']['ok'] == 1) {

            // แสดง popup ให้กลับไปหน้า login ด้วย account ที่สร้างขึ้น
            showDialog(
                    context: context,
                    builder: (BuildContext context) {
                    return AlertDialog(
                        title: Text('Success!'),
                        content: Text(
                            'Registration success! Please login with your account.'),
                        actions: [
                        TextButton(
                            onPressed: () {
                            Navigator.pop(context);
                            Navigator.pop(context);
                            },
                            child: Text('ok'),
                        )
                        ],
                    );
                    },
                );
        } else {
            // ถ้ามีบางอย่างผิดพลาด ก็แสดง pop up เหมือนกัน
            showDialog(
                context: context,
                builder: (BuildContext context) {
                    return AlertDialog(
                        title: Text('Opps...'),
                        actions: [
                        TextButton(
                            child: Text('close'),
                            onPressed: () {
                            Navigator.pop(context);
                            },
                        )
                        ],
                    );
                },
            );
        }

    }
}
```

## ฟังก์ชั่นเต็ม onPressed:

```dart
// lib/pages/signup_page.dart

onPressed: () async {
if (_formKey.currentState.validate()) {
    _formKey.currentState.save();

    var data = {"email": _email, "password": _password};

    final response = await Connection.getDio().post(
        '/signup',
        data: json.encode(data),
    );

    if (response.statusCode == 200 &&
        response.data['user']['ok'] == 1) {
            showDialog(
                context: context,
                builder: (BuildContext context) {
                return AlertDialog(
                    title: Text('Success!'),
                    content: Text(
                        'Registration success! Please login with your account.'),
                    actions: [
                    TextButton(
                        onPressed: () {
                        Navigator.pop(context);
                        Navigator.pop(context);
                        },
                        child: Text('ok'),
                    )
                    ],
                );
                },
            );
    } else {
        showDialog(
            context: context,
            builder: (BuildContext context) {
                return AlertDialog(
                    title: Text('Opps...'),
                    actions: [
                    TextButton(
                        child: Text('close'),
                        onPressed: () {
                        Navigator.pop(context);
                        },
                    )
                    ],
                );
            },
        );
    }
}
},

```