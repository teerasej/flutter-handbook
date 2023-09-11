
# สร้างหน้าลงชื่อเข้าใช้

ในกรณีที่ผู้ใช้ต้องการ sign in เข้าใช้งาน จะเป็นหน้าที่ของหน้า sign in ซึ่งมีลำดับดังนี้

1. ส่ง email และ password ไปที่ Web API
2. Web API จะตรวจเช็ค
3. ถ้ามี account อยู่จะส่ง token กลับมาให้
4. แอพบันทึก token เก็บไว้เพื่อใช้ในกระบวนการเข้าถึงข้อมูลส่วนอื่น

## 1. ดึงข้อมูลจาก Form มาสร้างเป็น Object

```dart
// lib/pages/signin_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"email": _email, "password": _password};

    }
}
```

## 2. ส่ง POST Request ไปที่เว็บ API เพื่อลงชื่อเข้าใช้งาน

```dart
// lib/pages/signin_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"email": _email, "password": _password};
        
        // เรียกใช้ Dio.post() เพื่อส่ง post request และรับ response กลับจาก web api
        final response = await Connection.getDio().post(
            // url สำหรับลงชื่อเข้าใช้งาน
            '/login',
            // ใช้ json.encode() เพื่อแปลงข้อมูลให้อยู่ในรูปแบบ JSON format
            data: json.encode(data),
        );

    }
}
```

## 3. เช็คค่าและจัดการ token

```dart
// lib/pages/signin_page.dart

onPressed: () async {
    if (_formKey.currentState.validate()) {
        _formKey.currentState.save();

        var data = {"email": _email, "password": _password};

        final response = await Connection.getDio().post(
            '/signin',
            data: json.encode(data),
        );

        // check ข้อมูลจาก Web API
        // ในที่นี้ถ้า statueCode เป็น 200 และค่า token ติดมาด้วย (เทียบแล้วไม่ใช่ค่า null) ถือว่าลงชื่อเข้าใช้แล้ว
        if (response.statusCode == 200 &&
            response.data['token'] != null) {

            // บันทึก token ที่ได้ลงในเครื่องผ่าน Token Manager
            TokenManager.saveToken(response.data['token']);

            // ถ้าผู้ใช้ไปหน้าแอพแสดงข้อมูลของตัวเอง
            Navigator.pushReplacement(
                context,
                MaterialPageRoute(
                builder: (BuildContext context) {
                    return NotePage();
                },
                ),
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
// lib/pages/signin_page.dart

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