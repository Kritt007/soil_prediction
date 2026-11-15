# เวอร์ชันเก่า (v1) — แผนที่รายสถานีอากาศ

ไฟล์ทั้งหมดในโฟลเดอร์นี้คือ repo ก่อนจัดโครงสร้างใหม่ (15 พ.ย. 2569) เก็บไว้อ้างอิงเท่านั้น
- `03_thailand_crop_map.ipynb` — Gaussian Naive Bayes, ดินจาก soil.xml (19 จังหวัด)
- `03_thailand_crop_map_xgboost.ipynb` — XGBoost รุ่นเก่า (`enriched_crop_pipeline.pkl` ในโฟลเดอร์นี้ = XGBoost 59 features)
- `04_thailand_crop_map_xgboost_sisoil.ipynb` — XGBoost รุ่นเก่า + ดิน SISoil

notebook เหล่านี้รันได้ภายในโฟลเดอร์นี้ (ไฟล์ที่ต้องใช้อยู่ครบ) แต่ **ไม่ได้ใช้โมเดลล่าสุดของเพื่อน**
ใช้ `notebooks/03_crop_suitability_map.ipynb` แทน
