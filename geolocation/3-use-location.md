
# เรียกใช้งาน Location 


## 1. Import package

เราจะเรียกใช้ location package ในไฟล์ `lib/main.dart`

```dart
// lib/main.dart
import 'package:location/location.dart';
```

## 2. Property สำหรับแสดงค่าตำแหน่งบนหน้าแอพ

สังเกตว่าจะมี Property ตัวหนึ่งที่ถูกใช้ในการแสดงข้อความขึ้นมาบนหน้าแอพ ถ้าได้ตำแหน่งละติจูด ลองจิจูดแล้ว เราจะเอามาแสดงตรงนี้

```dart
var _currentLocationText = "...";

Text('$_currentLocationText')
```

## 3. เรียกใช้ Location เพื่อขอพิกัด

จะมีปุ่ม 

```dart
child: Text('ขอพิกัดตอนนี้'),
onPressed: () async {

    // เรียกใช้งาน location 
    var location = Location();
    // ขอตำแหน่ง location ปัจจุบัน และเก็บไว้ในตัวแปรชื่อ currentLocation
    var currentLocation = await location.getLocation();

    // เรียกข้อมูล latitude และ longitude จากตัวแปรชื่อ currentLocation
    var locationText =
        '${currentLocation.latitude}, ${currentLocation.longitude}';

    // แสดงข้อความใน debug console
    print(locationText);

    // setState เพื่อกำหนดค่าให้กับ property _currentLocationText ให้แสดงค่าขึ้นมาบนหน้าจอ
    setState(() {
        _currentLocationText = locationText;
    });
},
```

onPressed: () async {

    var location = Location();
    var currentLocation = await location.getLocation();

    var locationText = '${currentLocation['latitude']} ${currentLocation['longitude']}';

    print(locationText);
    setState(() {
        _currentLocationText = locationText;
    });
},
```