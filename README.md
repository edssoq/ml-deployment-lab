# California Housing ML Service

เว็บและ API สำหรับทำนายค่ามัธยฐานของราคาบ้านในแคลิฟอร์เนียด้วย Flask และ Random Forest ผลทำนายมีหน่วยเป็นแสนดอลลาร์สหรัฐ

## Build และ Run ด้วย Docker

เปิด Docker แล้วรันคำสั่งในโฟลเดอร์ที่มี `Dockerfile` ขั้นตอน build จะดาวน์โหลดชุดข้อมูล California Housing และเทรนโมเดลโดยอัตโนมัติ

```powershell
docker build -t ds-ml-service .
docker run --rm -p 5000:5000 ds-ml-service
```

## ทดสอบการทำงาน

1. เปิด <http://127.0.0.1:5000/> แล้วกด **Use sample values** และ **Predict** ผลควรอยู่ที่ประมาณ `4.029` หรือประมาณ 403,000 ดอลลาร์สหรัฐ
2. เปิด <http://127.0.0.1:5000/health> แล้วตรวจว่าได้ `ok: true`
3. ทดสอบ API โดยเปิด PowerShell อีกหน้าต่างแล้วรันคำสั่งนี้

```powershell
Invoke-RestMethod -Uri http://127.0.0.1:5000/predict -Method Post -ContentType 'application/json' -Body '{"features":[8.3,25,6,1,1200,3,34.2,-118.3]}'
```

กด `Ctrl+C` ในหน้าต่างที่รัน container เมื่อต้องการหยุดบริการ
