
# Extension Method 


Extension Method เป็นการเพิ่มความสามารถ (เพิ่ม method ใหม่นั่นแหละ) ให้กับ Class ที่มีอยู่แล้ว ทำให้เราสามารถเรียกใช้ method เพิ่มเติมจากการเรียกใช้ Class ปกติได้ 

## สร้าง extension method 

สร้างไฟล์ **extension_buddhist_datetime.dart**

```dart
extension BuddhistCalendarDateTime on DateTime {

  // เพิ่ม method สำหรับปีในรูปแบบพุทธศักราช โดยอิงจากข้อมูล year ของ DateTime
  int get yearInBuddhistCalendar {
    return this.year + 543;
  }
}
```

สร้างไฟล์ **extension_buddhist_dateformat.dart**

```dart
extension BuddhistCalendarFormatter on DateFormat {

  // เพิ่ม method สำหรับ format DateTime ในรูปแบบพุทธศักราช
  String formatInBuddhistCalendarThai(DateTime dateTime) {
    if (this.pattern.contains('y')) {
      var buddhistDateTime = DateTime(
          dateTime.year,
          dateTime.month,
          dateTime.day,
          dateTime.hour,
          dateTime.minute,
          dateTime.second,
          dateTime.millisecond,
          dateTime.microsecond);

      // This path check DateFormat's locale, which already support Thai's one.
      // If you want to contribute, there are several countries that also use Buddhist calendar.
      // So you can help me correct their format by update the code below.
      // Reference: https://en.wikipedia.org/wiki/Buddhist_calendar
      // - Thailand
      // - Cambodia
      // - Laos
      // - MyanMar
      // - Sri Lanka
      // - Malaysia
      // - Singapore
      if (this.locale.contains('th') || this.locale.contains('TH')) {
        var normalYear = buddhistDateTime.year;
        var dateTimeString =
            this.format(buddhistDateTime).replaceAll('ค.ศ.', 'พ.ศ.');
        dateTimeString = dateTimeString.replaceAll(
            normalYear.toString(), (normalYear + 543).toString());
        return dateTimeString;
      } else {
        var result = this.format(buddhistDateTime);
        return result;
      }
    }
    return this.format(dateTime);
  }
}
```