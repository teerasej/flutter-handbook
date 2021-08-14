
# ทดสอบเพิ่มข้อมูลลง Database ผ่าน Mongo Compass

1. กลับมาที่ Mongo Compass และทำการเชื่อมต่อฐานข้อมูลให้เรียบร้อย 
2. สร้างฐานข้อมูลใหม่ โดยการกดปุ่ม Create Database

<img width="642" alt="2021-08-14_12-47-50" src="https://user-images.githubusercontent.com/85179/129448854-27156911-ad55-48d5-a039-a54af6e00d05.png">


3. ตั้งค่าตามภาพ และกดปุ่ม Create Database 

- **Database Name**: newsDB
- **Collection Name**: news 

<img width="606" alt="2021-08-14_12-51-22" src="https://user-images.githubusercontent.com/85179/129448863-973a3e4c-230d-4550-95fb-d22c9992152e.png">


4. จะเห็นว่ามีชื่อฐานข้อมูลแสดงขึ้นมาในรายการ กดเลือกฐานข้อมูล

<img width="536" alt="2021-08-14_12-51-36" src="https://user-images.githubusercontent.com/85179/129448870-3a81b215-b47a-426b-9a63-3dd6983463c3.png">


5. เมนูด้านซ้าย จะแสดงให้เห็นว่าเรากำลังเลือกฐานข้อมูลตัวไหนอยู่ และแสดง collection ขึ้นมาในรายการ ให้กดเลือก Collection **news**

<img width="747" alt="2021-08-14_12-51-46" src="https://user-images.githubusercontent.com/85179/129448877-1c6650fa-ac30-4be0-9998-06569cb475df.png">


6. จะเข้ามาสู่หน้าแสดงข้อมูล (อีกชื่อคือ Document) ที่อยู่ภายใน news collection ซึ่งตอนนี้เป็น collection เปล่าๆ 

ให้กดปุ่ม **Add Data > Insert Document** เพื่อเพิ่มข้อมูลด้วยตัวเอง

<img width="429" alt="2021-08-14_12-52-20" src="https://user-images.githubusercontent.com/85179/129448897-9a73b1e3-e6a0-4de3-ba55-15ed597b8edd.png">


7. จะมีการแสดงป๊อปอัพเป็นชุดข้อมูล ของ Document ที่จะถูกสร้างขึ้นใหม่ เราจะทำการเพิ่มข้อมูลชื่อ content เข้าไปใน document 
   1. กดปุ่มแสดงข้อมูลแบบ column
   2. กดปุ่มเพื่อเพิ่ม field ใหม่
   3. เลือก **Add Field After _id**

<img width="611" alt="2021-08-14_12-52-37" src="https://user-images.githubusercontent.com/85179/129448902-5a472595-e122-4a3e-a230-f94a6502afd9.png">


8. เพิ่มข้อมูล field 
   1. กรอกชื่อ field ว่า **content** และข้อความเนื้อข่าวลงไป
   2. กดปุ่ม **insert** เพื่อยืนยันข้อมูล

<img width="596" alt="2021-08-14_12-52-59" src="https://user-images.githubusercontent.com/85179/129448907-b2d7c421-235a-4c33-b929-b2576feaedae.png">


9. Document ใหม่จะแสดงขึ้นมาในรายการ

<img width="1034" alt="2021-08-14_13-53-52" src="https://user-images.githubusercontent.com/85179/129448933-eda16376-6a85-41c1-aed9-0f80435c0ac0.png">
