
# การ Setup ให้พร้อมใช้งาน firebase_auth

## 1. ติดตั้ง Package firebase_auth

จากนั้นติดตั้ง plugin `firebase_auth` ด้วยคำสั่งด้านล่าง

```
flutter pub add firebase_auth
flutterfire configure
```

## 2. Login เข้าไปใน Firebase console 

1. login เข้าไปใน [Firebase console](https://console.firebase.google.com/)
2. สร้างหรือเลือกโปรเจคที่ต้องการ
3. เข้าไปที่ส่วน Authentication

<img width="768" alt="2023-09-13_21-30-12" src="https://github.com/teerasej/react-native-handbook/assets/85179/8ab3f78b-39f1-413b-a8be-c1cdee7303b6">

4. ถ้ายังไม่ได้เปิดการทำการจะต้องเปิดก่อน

<img width="699" alt="2023-09-13_21-30-28" src="https://github.com/teerasej/react-native-handbook/assets/85179/07a11cc6-7770-47b3-90ba-a9b6c9457ed6">

5. เลือก Sign-in method และเลือก Email password

<img width="642" alt="2023-09-13_21-30-45" src="https://github.com/teerasej/react-native-handbook/assets/85179/53f68a50-7fe0-48bc-8291-7329c7df2872">

4. กดเปิดการทำงานของ Email/Password

<img width="1012" alt="2023-09-13_21-31-04" src="https://github.com/teerasej/react-native-handbook/assets/85179/9746957c-c770-4545-be38-ad4b615e8ab2">

5. รายการที่เปิดแล้ว จะได้ในรายการดังภาพ

<img width="1004" alt="2023-09-13_21-31-17" src="https://github.com/teerasej/react-native-handbook/assets/85179/045e589a-1963-46a0-9359-3511a619ceb6">
