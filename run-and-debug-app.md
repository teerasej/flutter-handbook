
## วิธีสร้างโปรเจค Flutter ใน Visual Studio Code

1. เปิดโปรแกรม Visual Studio Code
2. เรียกใช้งาน Command Palette โดยไปที่เมนู **View > Command Palette**
3. เรียกคำส่ัง **Flutter: New Project**
4. ตั้งชื่อโปรเจคในกล่องข้อความให้เรียบร้อย (img) 
5. เลือกโฟลเดอร์ที่จะสร้างโปรเจคเก็บไว้
6. รอจนระบบสร้างโปรเจคจนเสร็จ

## วิธีรันทดสอบแอพพลิเคชั่นจาก Visual Studio Code

เราสามารถสั่ง Build และรันแอพพลิเคชั่นได้ทันที โดยกดปุ่ม **F5**

หรือสามารถเริ่มต้นโดยเข้าจากโหมด Debug ก็ได้ 
1. เลือกโหมด **Debug** (img) 
2. จากเมนู Dropdown ที่แสดงว่า **No Configuration** ให้เลือกคำสั่ง **Add Configuration** (img) 
3. หากเป็นครั้งแรกจะมีการดาวน์โหลดระบบเพิ่มเติมมาจาก Internet เสร็จแล้วจะมีการสร้างไฟล์ `launch.json` ซึ่งจะเป็นการตั้งค่าสำหรับโปรเจค Flutter (img) 
4. เปิด Command Palette และใช้คำสั่ง **Flutter: Select Device** เพื่อเลือกชื่ออุปกรณ์ที่เชื่อมต่อกับคอมพิวเตอร์ หรือ Simulator ที่ต้องการ (img)

5. กดปุ่ม **Start Debug**