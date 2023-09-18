
# Setup Deep Link for Android
 
## ปรับเพิ่ม <intent-filter> ในไฟล์ AndroidManifest.xml

```xml
<!-- android/app/src/main/AndroidManifest.xml -->
...

    <activity android:name=".MainActivity" ...>
            ...

            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />

                <!-- กำหนดให้ตอบสนองต่อ scheme, host, และ path -->
                <!-- http://nextflow.in.th/news -->
                <!-- https://nextflow.in.th/news -->
                <!-- https://nextflow.in.th/news/detail/2 -->
                <data android:scheme="https" android:host="nextflow.in.th" android:pathPrefix="/news" />
                 <data android:scheme="http" />
            </intent-filter>

            <!-- กำหนดให้ตอบสนองต่อ scheme, host แบบ custom -->
                <!-- fastnews://news -->
                <!-- fastnews://news/news/detail/2 -->
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="fastnews" android:host="news" />
            </intent-filter>
        </activity>

...
```
