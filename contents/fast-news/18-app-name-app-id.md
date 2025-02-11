
# กำหนดชื่อแอพ และ appId

ในตอนต้น ตัว flutter จะกำหนด package และชื่อแอพ (app ID) เริ่มต้นมาให้ ซึ่งการเปลี่ยนมันภายหลังจะมีความยุ่งยากพอสมควร เราจึงมีการใช้ package อื่นๆ มาช่วย 

### A. [rename](https://pub.dev/packages/rename)

รันคำสั่งดังต่อไปนี้เพื่อทำการเปลี่ยนแปลง

1. ติดตั้ง

```bash
flutter pub global activate rename
```

2. เปลี่ยนชื่อแอพ

```bash
rename setAppName --targets ios,android --value "MyAppName"
```
หรือ
```bash
flutter pub global run rename setAppName --targets ios,android --value "MyAppName"
```

3. เปลี่ยน package name (App ID ของฝั่ง android)

```bash
rename setBundleId --targets android --value "th.in.nextflow.MyAppName"
```
หรือ 
```bash
flutter pub global run rename setBundleId --targets android --value "th.in.nextflow.MyAppName"
```

4. เปลี่ยน bundleId (App ID ของฝั่ง iOS)

```bash
rename setBundleId --targets ios --value "th.in.nextflow.MyAppName"
```
หรือ
```bash
flutter pub global run rename setBundleId --targets ios --value "th.in.nextflow.MyAppName"
```

### B. [change_app_package_name](https://pub.dev/packages/change_app_package_name)

ในกรณีที่เราต้องการเปลี่ยนโครงสร้างของโปรเจค Android ให้สอดคล้องกับ package name (App ID ในฝั่ง Android) เราสามารถใช้ package นี้ช่วยด้วย

1. ติดตั้งโดยการเพิ่มชื่อ Package เข้าไปในส่วน `dev_dependencies`

```yaml
dev_dependencies: 
  change_app_package_name: ^1.1.0
```

2. ใช้คำสั่งใน terminal เพื่อเปลี่ยน package name รวมถึง โครงสร้างของ directory ใน `android/app`

```bash
flutter pub run change_app_package_name:main th.in.nextflow.fastnews
```
