
# Fast News - Day 3

- [white board](https://teerasej440384.invisionapp.com/freehand/Flutter-Special---The-Nation-group-ctuTBjBUN)

สร้างโปรเจคใหม่ชื่อ 

```bash
fast_news_app
```

## Part 1 - Presentation layer และ widget

1. [ติดตั้ง **GetX** State Management](1-setup-getx.md) 
2. [สร้าง News Page ด้วย ListView ](2-create-news-page.md)
3. [สร้าง News Detail Page](3-create-news-detail.md)

## Part 2 - domain entity กับ data layer

4. [ตัวอย่าง json](4-json-example.md) 
5. [การสร้าง Model Class ด้วย JsonToDart VSCode Extension](5-json-to-dart.md)
6. [สร้าง Data Repository สำหรับจัดการโค้ดส่วนติดต่อกับ Web API](6-create-data-repository.md)

## Part 3 - Domain use case และ Controller

8. [สร้าง Use case ที่เป็น action ที่สั่งโหลดข้อมูล](8-create-use-case.md)
9. [สร้าง News Controller ที่เรียกใช้ Use case ต่างๆ](9-create-news-controller.md)
10. [นำ News Controller มาเชื่อมการทำงานบน News Page](10-connect-controller.md)
11. [เปิดดูรายละเอียดข่าว](11-open-news-detail.md)
12. [ย้ายส่วนของการสร้าง Controller ไปไว้ใน Main](12-move-controller-to-main.md)

## Part 4 - Form และ Controller

13. [สร้าง Sign In page และ Sign up page พร้อมแบบฟอร์ม](13-create-sign-in-page.md)
14. [เปิดหน้า Sign up page จากหน้า Sign in](14-open-create-account.md) 
15. [สร้าง Controller เพื่อใช้งานกับแบบฟอร์มใน Sign in และ Sign up page](15-auth-controller.md)
16. [นำ AuthController มาใช้งานกับหน้า Sign in และ Sign up page](16-setup-auth-controller.md)

## Part 5 - เชื่อมต่อ Web API 

17. [สร้างส่วน News Repo Web API ใช้งาน](17-get-connect.md)

## Part 6 - เชื่อมต่อ Firebase

18. [กำหนดชื่อแอพ และ appId](18-app-name-app-id.md)
19. [Setup Firebase](19-setup-firebase.md)
20. [สร้าง Sign in use case](20-sign-in-use-case.md)
21. [สร้าง Sign up use case](21-sign-up-use-case.md)
22. [สร้างกลไกเช็คการ Sign in ของ user](22-validate-user-sign-in.md)
23. [สร้าง และเรียกใช้ Sign out Use case](23-sign-out-use-case.md)

## Part 7 - Deep Links - Android

24. [สร้างไฟล์ assetlinks.json](24-create-assetlinks.md)
25. [Setup Android's Deep Link](25-setup-android-deep-link.md)

## Part 8 - Deep Links - iOS

26. [A. Setup iOS's Custom Scheme](26-setup-ios-deep-link.md)
27. [ฺB. Setup iOS's Universal Link](27-apple-app-site-association.md)

## Part 9 - Deep Links - Flutter 

28. [Setup และ ใช้งาน App Link](28-app-links.md)
29. [ปรับให้ News Detail ตอบสนองต่อการเปิด AppLink](29-news-detail-app-link.md)