# soil_prediction — แผนที่พื้นที่เหมาะกับพืชในประเทศไทย

นำโมเดลแนะนำพืช 22 ชนิดจากโปรเจค [Project_CropRecommendation](https://github.com/6631501036-dev/Project_CropRecommendation)
มาทำนายกับ **ผลวิเคราะห์ดินจริงทั่วประเทศ** (~126,000 จุด 77 จังหวัด) แล้วแสดงเป็นแผนที่รายอำเภอ

## โครงสร้างโฟลเดอร์

```text
soil_prediction/
├── notebooks/
│   └── 03_crop_suitability_map.ipynb   # notebook หลัก (รันไฟล์นี้)
├── models/
│   ├── enriched_crop_pipeline.pkl      # โมเดลล่าสุดของเพื่อน: Random Forest, 23 features (15 พ.ย. 2569)
│   └── crop_recommendation_pipeline.pkl # โมเดล baseline: Gaussian Naive Bayes, 7 features
├── data/
│   ├── raw/                            # ข้อมูลต้นทาง
│   │   ├── Crop_recommendation.csv     #   ชุดข้อมูล Kaggle (2,200 แถว)
│   │   ├── climate_normal.xml          #   ค่าปกติภูมิอากาศ กรมอุตุฯ 1981–2010
│   │   └── soil.xml                    #   ผลวิเคราะห์ดิน 1,000 รายการล่าสุด (ไม่ได้ใช้ในเวอร์ชันนี้)
│   ├── reference/                      # ข้อมูลอ้างอิง
│   │   ├── enriched_crop_dataset_cleaned.csv  # ชุด train ของเพื่อน (ใช้ประเมินโมเดล)
│   │   ├── thailand_actual_crop_area.csv      # พื้นที่ปลูกจริงรายจังหวัด (สศก.)
│   │   └── thailand_provinces.geojson         # ขอบเขตจังหวัด
│   └── soil/                           # ดินทั่วประเทศ — notebook ดาวน์โหลดให้เอง (ไม่อยู่ใน git)
├── outputs/                            # ผลลัพธ์ที่ notebook สร้าง
│   ├── crop_suitability_map.html       #   แผนที่แบบมีเมนู เปิดในเบราว์เซอร์ได้
│   ├── amphoe_crop_suitability.csv     #   % จุดตรวจที่เหมาะกับแต่ละพืช รายอำเภอ
│   ├── model_test_metrics.csv          #   Accuracy / Precision / Recall / F1 บนชุดทดสอบ
│   └── model_check_actual_area.csv     #   F1 เทียบพื้นที่ปลูกจริง
└── archive/v1_station_map/             # เวอร์ชันเก่าทั้งหมด (เก็บไว้อ้างอิง)
```

## วิธีรัน
1. เปิด JupyterLab ที่โฟลเดอร์ `soil_prediction`
2. เปิด `notebooks/03_crop_suitability_map.ipynb` เลือก kernel Python 3 แล้ว Run All
3. ครั้งแรกต้องต่ออินเทอร์เน็ต (ดาวน์โหลดข้อมูลดิน ~35 MB) ใช้เวลาราว 1 นาที

## เมื่อเพื่อนอัปเดตโมเดล
คัดลอก `enriched_crop_pipeline.pkl` ตัวใหม่มาวางทับใน `models/` และ `data/enriched_crop_dataset_cleaned.csv` ตัวใหม่ไปที่ `data/reference/` แล้วรันใหม่
notebook สร้างคอลัมน์ตาม `feature_names` ในไฟล์โมเดลเอง จึงไม่ต้องแก้โค้ด ถ้าเพื่อนยังใช้ชุดคอลัมน์แบบเดิม

## ผลประเมิน (Random Forest, 15 พ.ย. 2569)

| ตัววัด | ผล |
|---|---|
| Accuracy / Macro F1 บนชุดทดสอบ (440 แถว) | 0.9955 / 0.9955 |
| Top-3 hit rate บนชุดทดสอบ | 1.00 |
| F1 เฉลี่ยเทียบพื้นที่ปลูกจริง (ระดับจังหวัด, 5 รายการ) | 0.14 |

## แหล่งข้อมูล
- ผลวิเคราะห์ดิน: กรมพัฒนาที่ดิน, การตรวจวิเคราะห์ดิน (osd01), CC BY — รวบรวมโดย [SISoil](https://github.com/Navavit/sisoil)
- ภูมิอากาศ: กรมอุตุนิยมวิทยา
- พื้นที่ปลูกจริง: สำนักงานเศรษฐกิจการเกษตร ([catalog.oae.go.th](https://catalog.oae.go.th))
- ชุดข้อมูลพืช: [Kaggle Crop Recommendation Dataset](https://www.kaggle.com/datasets/atharvaingle/crop-recommendation-dataset)
