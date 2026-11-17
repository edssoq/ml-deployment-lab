# California Housing ML Service

เว็บและ API สำหรับทำนายค่ามัธยฐานของราคาบ้านในแคลิฟอร์เนียด้วย Flask และ Random Forest ผลทำนายมีหน่วยเป็นแสนดอลลาร์สหรัฐ

## 1. สร้าง Docker image

เปิด Docker Desktop และรอให้ขึ้น **Engine running** จากนั้นเปิด Terminal ในโฟลเดอร์ `ml_service` ที่มี `Dockerfile` แล้วรัน

```sh
docker build -t ds-ml-service .
docker images ds-ml-service
```

ระหว่าง build สคริปต์ `train_model.py` จะดาวน์โหลดชุดข้อมูล California Housing เทรนโมเดล และบันทึก `model/model.pkl` ไว้ใน image เมื่อสำเร็จจะเห็น `ds-ml-service:latest` ในรายการ image

## 2. รัน container

```sh
docker run --rm -p 5000:5000 ds-ml-service
```

เมื่อ Terminal แสดง `Listening at: http://0.0.0.0:5000` ให้เปิด Terminal ค้างไว้ระหว่างทดสอบเว็บ กด `Ctrl+C` ใน Terminal นี้เมื่อต้องการหยุด container

## 3. ทดสอบหน้าเว็บและ API

- **หน้าเว็บ** เปิด <http://127.0.0.1:5000/> กด **Use sample values** แล้วกด **Predict** ค่าตัวอย่างจะแสดง `Prediction: 4.029` หรือประมาณ 403,000 ดอลลาร์สหรัฐ
- **สถานะบริการ** เปิด <http://127.0.0.1:5000/health> แล้วตรวจว่าผล JSON มี `"ok": true`
- **API ทำนาย** เปิด PowerShell อีกหน้าต่างแล้วส่งค่าตัวอย่างทั้ง 8 ค่าไปที่ `POST /predict`

```powershell
Invoke-RestMethod -Uri 'http://127.0.0.1:5000/predict' -Method Post -ContentType 'application/json' -Body '{"features":[8.3,25,6,1,1200,3,34.2,-118.3]}'
```

API จะคืน JSON ที่มี `prediction` ประมาณ `4.02947` โดยรับค่า `features` ตามลำดับ `MedInc`, `HouseAge`, `AveRooms`, `AveBedrms`, `Population`, `AveOccup`, `Latitude`, `Longitude`
