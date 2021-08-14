
# ติดตั้ง ngrok 


## 1. ติดตั้ง Node.js 

ติดตั้ง Node.js 

- ดาวน์โหลดตัวติดตั้ง
- [ดูคลิปแนะนำขั้นตอนการติดตั้ง](https://www.youtube.com/watch?v=EOeDDoU4A3M)

## 2. ติดตั้ง ngrok

ให้เปิดโปรแกรม Command Prompt (ถ้าใช้งาน Windows) หรือ Terminal (ถ้าใช้งาน MacOS)

จากนั้นพิมพ์ และรันคำสั่ง

```bash
npm install -g ngrok
```

## 3. ทดสอบการใช้งาน


ให้เปิดโปรแกรม Command Prompt (ถ้าใช้งาน Windows) หรือ Terminal (ถ้าใช้งาน MacOS)

จากนั้นพิมพ์ และรันคำสั่ง

```bash
ngrok http 8080
```

น่าจะเห็นข้อความประมาณด้านล่างแสดงขึ้นมา ถือว่า ngrok ทำงานได้อย่างสมบูรณ์

```bash
ngrok by @inconshreveable                                                                                                                      (Ctrl+C to quit)
                                                                                                                                                               
Session Status                online                                                                                                                           
Session Expires               1 hour, 59 minutes                                                                                                               
Version                       2.3.40                                                                                                                           
Region                        United States (us)                                                                                                               
Web Interface                 http://127.0.0.1:4040                                                                                                            
Forwarding                    http://addbef879084.ngrok.io -> http://localhost:3000                                                                            
Forwarding                    https://addbef879084.ngrok.io -> http://localhost:3000                                                                           
                                                                                                                                                               
Connections                   ttl     opn     rt1     rt5     p50     p90                                                                                      
                              0       0       0.00    0.00    0.00    0.00        
```