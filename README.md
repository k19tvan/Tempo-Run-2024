# **TECHNICAL REPORT**

## Vấn đề: Signboard Detection
## Model: Yolov9c
## Hướng tiếp cận:
- Với data ít (200 ảnh) sử dụng model yolov9c không quá mạnh để nhận diện trên bộ data ít tốt hơn.
- Sử dụng hai model yolo9c train trên hai tập data (Tập gốc và tập agment)
- Ensemble hai model cho ra kết quả tốt hơn
- Xoay những ảnh cần xoay trước khi inference.

## Chi tiết các bước:
- Bước 1: Tìm hiểu về data (Visualize data train để hiểu đúng về đề bài)
- Bước 2: Chọn model - Yolov9c
- Bước 3: Training
    - Model1:
        - Chia tập val tỉ lệ 1 / 5
        - Tunning 100 lần, mỗi lần 60 epoch trên tập dữ liệu gốc để chọn ra bộ tham số phù hợp:
        - Train 200 epochs và save period mỗi 10 epochs để quan sát
        - Chọn được epoch 150 là checkpoint tốt nhất
    - Model2:
        - Bỏ 14 ảnh ít thông tin qua tập val
        - Augment data train, với những ảnh có thông tin quan trọng thì augment ra thêm 6 ảnh, còn không thì thêm 2 ảnh.
        - Vẫn dùng bộ tham số cũ và save period mỗi 10 epochs để quan sát.
        - Chọn được epoch 10 là checkpoint tốt nhất.
- Bước 4: Tiền xử lý dữ liệu test
    - Chọn những ảnh bị rotate left để rotate right trước khi inference
- Bước 5: Ensemble model và inference
    - Ensemble model 1 và model 2, inference, rồi rotate left lại các labels của các ảnh được rotate right  
