
# จัดการหน้าแอพตอนเริ่มทำงาน

ในที่นี้เราจะทำการเช็คของเปิดแอพว่า ผู้ใช้เคย login เข้าระบบสำเร็จไหม โดยการเช็คจาก token 

- ถ้าไม่พบ token ถือว่าผู้ใช้ยังไม่ได้ login ให้เปิดหน้า login 
- แต่ถ้าพบ token ก็แสดงหน้าแอพให้ใช้งานได้ปกติ

จะมีการใช้ FutureBuilder ในการจัดการขั้นตอนของการทำงานแบบ Asynchronous หรือที่ในภาษา Dart คือ Class ชื่อ Future

```dart
// main.dart

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return FutureBuilder(
      // กำหนดให้ Future Builder ดูการทำงานของ Future ที่ได้จาก TokenManager.isTokenExist()
      future: TokenManager.isTokenExist(),
      builder: (BuildContext context, AsyncSnapshot<bool> snapshot) {

        // เช็ค snapshot.connectionState ซึ่งถ้าค่า future ทำงานสมบูรณ์แล้ว จะมีการเท่ากับ ConnectionState.done
        if (snapshot.connectionState == ConnectionState.done) {

          // snapshot.data คือผลลัพธ์ที่ได้จาก future: ในที่นี้เป็น boolean สามารถเอามาเช็คได้  
          if (snapshot.data) {
            // ถ้าค่าเป็น true แปลว่ามี token อยู่ ให้เข้าไปที่หน้าแสดงข้อมูล NotePage ได้เลย
            return NotePage();
          } else {

            // ถ้าค่าเป็น false แปลว่าไม่มี token เก็บอยู่ในเครื่อง ให้แสดงหน้า login 
            return LoginPage();
          }
        }

        // กรณีที่ future ที่กำหนดยังทำงานไม่สมบูรณ์ ค่า snapshot.connectionState จะไม่เท่ากับ ConnectionState.done
        // ให้แสดงตัวโหลดกลางหน้าจอแทน
        return Scaffold(
          appBar: AppBar(
            title: Text('Loading...'),
          ),
          body: Center(
            child: CircularProgressIndicator(),
          ),
        );
      },
    );
  }
}
```