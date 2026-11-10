# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Dương Quốc Huy
- MSSV: 2A202603311
- Email: duongquochuy1107@gmail.com
- Lớp: L2B
- Link repo bài làm: https://github.com/dqhuy11/K5-L2B-DAY02-DuongQuocHuy-2A202603311-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/1uTBrc8PV5yRNYBxxKFIF68wMtF1ZZffz#scrollTo=DlysQwtbYz7k

## 2. Schema và mini guideline

Tham khảo file [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

### 2.1. Schema áp dụng

Dataset sử dụng 4 lớp đối tượng trong bài toán Object Detection:

- `car`: Xe con.
- `truck`: Xe tải.
- `bus`: Xe buýt.
- `van`: Xe van.

Các lớp được sử dụng để xây dựng nhãn và huấn luyện mô hình YOLO26n.

### 2.2. Ba ca mơ hồ cần thống nhất

1. **Truck và van:** Một số xe tải nhỏ có hình dáng tương tự xe van, cần dựa vào đặc điểm phương tiện để phân loại thống nhất.
2. **Car và van:** Một số xe con có thiết kế thân xe dài, dễ nhầm với xe van.
3. **Truck và bus:** Khi phương tiện bị che khuất hoặc nhìn từ góc khó quan sát, việc phân biệt hai lớp có thể gặp khó khăn.

*Lưu ý: Notebook hiện tại chưa lưu lại ba quyết định annotation cụ thể đã được chốt.*

### 2.3. Phần cần giảng viên làm rõ

- Quy tắc phân biệt truck và van đối với các phương tiện có hình dáng tương tự.
- Cách xử lý đối tượng bị che khuất một phần.
- Quy định đối với đối tượng xuất hiện ở sát mép ảnh hoặc chỉ nhìn thấy một phần.

---

## 3. Annotation và self-QC

- **Số file nhãn train:** 50 file.
- **Kết quả checker:** Notebook kiểm tra thành công số lượng ảnh và nhãn giữa hai thư mục train và eval.
  - Train: 50 ảnh và 50 file nhãn.
  - Eval: 10 ảnh và 10 file nhãn.
  - Không có trường hợp thiếu file nhãn hoặc ảnh tương ứng.
- **Danh sách ảnh eval được hiển thị kết quả dự đoán:** `drive_051.jpg` đến `drive_060.jpg` (10 ảnh).
- **Lỗi bỏ sót/box/class phát hiện khi rà:** Notebook có hiển thị bounding box dự đoán trên 10 ảnh eval, nhưng chưa có ghi nhận cụ thể về lỗi annotation được phát hiện và sửa bằng mắt. Tuy nhiên, kết quả Gold QC ở mục 4 cho thấy nhãn train còn nhiều lỗi bỏ sót (FN) và box thừa (FP).
- **Commit khóa nhãn ban đầu:** Chưa xác định được commit khóa nhãn ban đầu. Commit mới nhất được ghi nhận khi chạy notebook là `50eea0e` (Add files via upload).

---

## 4. Gold QC và rework

File đánh giá: `outputs/annotation_evaluation.json` (ngưỡng IoU match = 0.5).

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 362 | Chưa có (chưa sửa nhãn) |
| FP | 261 | Chưa có |
| FN | 291 | Chưa có |
| Precision | 0.5811 | Chưa có |
| Recall | 0.5544 | Chưa có |
| F1 | 0.5674 | Chưa có |
| mean_iou_of_matches | 0.7559 | Chưa có |
| passed | false | Chưa có |

**Đối chiếu với ngưỡng yêu cầu:**

- F1 = 0.5674, thấp hơn mức yêu cầu 0.8, nên **không đạt**.
- mean_iou_of_matches = 0.7559, cao hơn mức yêu cầu 0.7, nên **đạt** tiêu chí này.
- Kết luận chung: `passed = false` vì F1 chưa đạt. Các box đã khớp thì khá chính xác về vị trí, vấn đề chính nằm ở **số lượng box**: bỏ sót nhiều (FN = 291, Recall 0.5544) và gán thừa nhiều (FP = 261, Precision 0.5811).

**Nhận xét theo từng ảnh (từ `per_image`):**

- Tổng số box gold là 653 (TP + FN), tổng số box của em là 623 (TP + FP), nên tổng số box gần bằng nhau nhưng khớp nhau chỉ khoảng 55%.
- Ảnh **bỏ sót nhiều**: `drive_041` (gold 24, em gán 9, khớp 4), `drive_045` (gold 15, em gán 8, khớp 3), `drive_040` (gold 19, em gán 11, khớp 5), `drive_020` (gold 8, em gán 3), `drive_015` (gold 12, em gán 6), `drive_024` (gold 30, em gán 19, khớp 12).
- Ảnh **gán thừa nhiều**: `drive_001` (gold 5, em gán 13), `drive_002` (gold 13, em gán 29), `drive_018` (gold 2, em gán 8, khớp 1), `drive_049` (gold 6, em gán 12).
- Ảnh **số box gần bằng nhưng khớp thấp**: `drive_008` (gold 16, em gán 18, khớp 6), `drive_009` (gold 21, em gán 18, khớp 6). Nhiều khả năng do box lệch hoặc khác phạm vi gán nhãn (đối tượng nhỏ, xa, bị che).
- File JSON không có thông tin theo lớp, nên chưa thể kết luận lỗi tập trung ở lớp nào.

**Rework:** Hiện chưa thực hiện sửa nhãn và chạy lại Gold QC, nên cột "Sau sửa" chưa có số liệu. Hướng sửa dự kiến: rà lại các ảnh bỏ sót/gán thừa nêu trên trước, sau đó chạy lại checker để so sánh F1 với mức yêu cầu 0.8.

---

## 5. Kết quả YOLO

### 5.1. Môi trường thực nghiệm

- Runtime: Google Colab.
- GPU: NVIDIA Tesla T4 (14.9 GB).
- Device: CUDA:0.
- Python: 3.13.16.
- Ultralytics: 8.4.208.
- PyTorch: 2.14.1+cu130.
- Model: YOLO26n (`yolo26n.pt`).

### 5.2. Tham số huấn luyện

| Tham số | Giá trị |
|---|---|
| Model | yolo26n.pt |
| Epochs | 50 |
| Image size | 640 |
| Batch size | -1 (AutoBatch) |
| Device | 0 (GPU) |
| Seed | 20260909 |
| Project | outputs/yolo26_runs |
| Name | vehicle_annotation |

Không thay đổi các tham số huấn luyện chính so với cấu hình được viết trong notebook. Notebook tự động xác định batch size phù hợp với GPU và sử dụng optimizer AdamW.

Ngoài ra, đường dẫn `path` trong `data.yaml` được chuyển thành đường dẫn tuyệt đối để đảm bảo Ultralytics có thể tìm thấy dataset.

### 5.3. Kết quả đánh giá trên 10 ảnh eval

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4101 |
| Recall | 0.2298 |
| mAP50 | 0.2719 |
| mAP50–95 | 0.2016 |

### 5.4. Kết quả theo từng lớp

| Class | Instances | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|---:|
| car | 94 | 0.833 | 0.636 | 0.738 | 0.523 |
| truck | 5 | 0.673 | 0.200 | 0.239 | 0.205 |
| bus | 2 | 0.000 | 0.000 | 0.00786 | 0.00393 |
| van | 12 | 0.134 | 0.0833 | 0.103 | 0.0742 |

### 5.5. Đường dẫn kết quả

- Best checkpoint: `outputs/yolo26_runs/vehicle_annotation/weights/best.pt`
- Kết quả đánh giá: `outputs/yolo26_runs/vehicle_eval/`
- Ảnh dự đoán: `outputs/eval_predictions/visuals/`
- File tổng hợp metric: `outputs/model_eval_metrics.json`
- File đánh giá nhãn (Gold QC): `outputs/annotation_evaluation.json`
- File kết quả Colab: `outputs/day2_colab_outputs.zip`

Các thư mục kết quả cần được giữ trong repository để có thể kiểm tra lại. Notebook hiện lưu chúng trong `outputs/`, chưa phải `assets/` như cấu trúc được hướng dẫn trong mẫu báo cáo.

### 5.6. Phân tích kết quả

Lớp `car` nhận diện tốt nhất (Recall 0.636, mAP50 0.738) vì có nhiều dữ liệu nhất (94 instances trong eval). Lớp `bus` có Recall 0.000 và `van` chỉ 0.0833, cho thấy mô hình gần như không nhận ra các lớp này.

Số lượng đối tượng trong tập eval không đồng đều: car 94, van 12, truck 5, bus 2. Với truck và bus, số mẫu quá ít nên chỉ số rất dao động, chỉ nên xem là tín hiệu tham khảo.

Kết quả Gold QC (F1 = 0.5674, chưa đạt 0.8) cho thấy chất lượng nhãn train cũng là một nguyên nhân có thể làm mô hình học chưa tốt, đặc biệt với dữ liệu train chỉ có 50 ảnh.

### 5.7. Trả lời bốn câu hỏi phân tích

**Câu 1. Lớp nào có recall thấp nhất? Ảnh nào minh họa?**

Lớp `bus` có Recall thấp nhất, bằng 0.000 (mAP50 chỉ 0.00786). Tập eval chỉ có 2 instances bus, nên Recall chỉ có thể nhận các giá trị rời rạc như 0, 0.5 hoặc 1 và rất nhạy với từng mẫu.

Ảnh minh họa: `drive_052.jpg`, vì mô hình dự đoán ra 1 box bus trên ảnh này. Với Recall = 0 và Precision = 0, box bus này không khớp với ground truth (sai vị trí, IoU dưới 0.5 hoặc sai lớp), và bus thật trong eval (nếu nằm ở ảnh khác) bị bỏ sót. Cần mở ảnh dự đoán đối chiếu với nhãn ground truth để xác nhận chính xác.

Hai lớp thấp tiếp theo là `van` (Recall 0.0833) và `truck` (Recall 0.200).

**Câu 2. Nếu mAP50 cao nhưng mAP50-95 thấp, box đang gặp vấn đề gì?**

Mô hình tìm đúng đối tượng (IoU ≥ 0.5) nhưng box chưa khít, nên điểm giảm mạnh khi ngưỡng IoU tăng dần lên 0.95. Ví dụ lớp `car`: mAP50 = 0.738 nhưng mAP50-95 = 0.523, chênh khoảng 0.21.

Cần kiểm tra trên box:
- Box có lệch tâm so với đối tượng không.
- Chiều rộng/chiều cao có bị quá rộng (chứa nền thừa) hoặc quá hẹp (cắt mất một phần xe) không.
- Box có nhất quán giữa các ảnh không (cùng một kiểu xe, cùng mức độ che khuất thì gán box giống nhau không).
- Với xe bị che hoặc sát mép ảnh: quy ước gán box cho phần nhìn thấy hay phần ước đoán.

Dữ kiện hỗ trợ: mean_iou_of_matches của nhãn train là 0.7559, chỉ hơn mức yêu cầu 0.7 một chút, nghĩa là vẫn có nhiều box khớp ở mức IoU trung bình (khoảng 0.5-0.75), sẽ bị loại ở các ngưỡng IoU cao hơn của mAP50-95.

**Câu 3. Có lỗi truck/bus/van/car nào liên quan tới guideline không?**

Có khả năng, tập trung ở `van`, `truck` và `bus`:

- `van`: Recall 0.0833, Precision 0.134. Đây là lớp có nhiều instance (12) nhưng kết quả thấp nhất ngoài bus, phù hợp với ca mơ hồ car/van và truck/van ở mục 2.2.
- `truck`: Recall 0.200 (5 instances), liên quan ca truck/van.
- `bus`: Recall 0.000 (2 instances), liên quan ca truck/bus khi bị che hoặc nhìn từ góc khó.

Dữ kiện hỗ trợ khác: Gold QC cho F1 = 0.5674, FP = 261, FN = 291, tức là nhãn của em chưa thống nhất với gold ở nhiều box. Tuy nhiên file Gold QC không chia theo lớp, và Recall thấp cũng có thể do dữ liệu ít, nên chưa thể kết luận nguyên nhân chắc chắn là gán sai lớp. Cần mở ảnh gốc, nhãn ground truth và box dự đoán để xác định lỗi cụ thể. Guideline cần bổ sung rõ quy tắc phân biệt van/truck/car và cách xử lý xe bị che.

**Câu 4. Vì sao không dùng 10 ảnh eval để train dù chúng có nhãn?**

Cần giữ riêng dữ liệu đánh giá để kiểm tra mô hình trên ảnh chưa từng được dùng để cập nhật trọng số. Nếu đưa 10 ảnh này vào train, mô hình có thể "học thuộc" chúng (data leakage), khiến Precision, Recall, mAP cao giả tạo và không phản ánh khả năng tổng quát hóa.

Trong notebook, 10 ảnh eval được khai báo ở phần `val` của `data.yaml`, được dùng để:

- Tính Precision, Recall, mAP50, mAP50-95 sau mỗi epoch.
- Theo dõi quá trình huấn luyện và chọn checkpoint `best.pt`.
- Đánh giá lại mô hình sau huấn luyện bằng `best_model.val()`.

**Lưu ý:** Vì 10 ảnh eval vừa dùng để chọn `best.pt` vừa dùng để báo cáo kết quả, chúng không phải một test set độc lập hoàn toàn. Ngoài ra, chỉ 10 ảnh nên số liệu của các lớp ít mẫu (truck, bus) không ổn định.

---

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Kết nối repository | Cần đảm bảo notebook sử dụng đúng repository đã chứa dataset và nhãn | Sử dụng Git clone/pull repository | Clone và pull thành công |
| Kiểm tra dataset | Cần kiểm tra số lượng ảnh và file nhãn | Sử dụng Python để đối chiếu tên file ảnh và nhãn | Đạt 50 ảnh train và 10 ảnh eval, các file nhãn tương ứng đầy đủ |
| Gold QC | F1 = 0.5674, chưa đạt 0.8; FP = 261, FN = 291 | Phân tích `per_image` để tìm ảnh bỏ sót/gán thừa nhiều | Đã xác định các ảnh cần rà lại (vd. drive_041, drive_045, drive_002); còn thiếu bước sửa nhãn và chạy lại |
| Huấn luyện YOLO | Cần lựa chọn batch phù hợp với GPU | Sử dụng `batch=-1` để AutoBatch tự xác định | GPU Tesla T4 sử dụng batch size 47 |
| Đánh giá mô hình | Kết quả giữa các lớp chưa đồng đều (bus, van thấp) | Sử dụng `best.pt` để đánh giá lại trên tập eval | Đã thu được các chỉ số Precision, Recall và mAP; còn thiếu rà lỗi lớp theo guideline |
| Kiểm tra trực quan | Cần quan sát bounding box trên ảnh thực tế | Sử dụng `model.predict()` và hiển thị 10 ảnh eval | Đã tạo ảnh dự đoán; cần đối chiếu chi tiết với ground truth |

---

## 7. Tự đánh giá theo rubric

Đối chiếu với [RUBRIC.md](../../RUBRIC.md), em tự đánh giá kết quả thực hiện như sau:

| Nội dung | Kết quả | Ghi chú |
|---|---|---|
| Xây dựng schema và mini guideline | Cần bổ sung | Chưa ghi lại đầy đủ ba quyết định mơ hồ |
| Gán nhãn 50 ảnh train | Đã xác nhận số lượng | Notebook xác nhận đủ 50 ảnh và 50 file nhãn |
| Kiểm tra dữ liệu | Đã thực hiện | Kiểm tra ảnh và nhãn tương ứng thành công |
| Self-QC bằng mắt | Chưa xác nhận đầy đủ | Đã hiển thị 10 ảnh eval nhưng chưa có biên bản rà lỗi |
| Gold QC | Đã có kết quả, chưa đạt | F1 = 0.5674 (yêu cầu 0.8); mean IoU = 0.7559 (đạt yêu cầu 0.7) |
| Rework nhãn | Chưa thực hiện | Cần sửa nhãn theo các ảnh có nhiều FN/FP rồi chạy lại Gold QC |
| Huấn luyện YOLO26n | Đã thực hiện | Hoàn thành 50 epochs |
| Đánh giá mô hình | Đã thực hiện | Có đầy đủ Precision, Recall, mAP50 và mAP50-95 |
| Kiểm tra trực quan | Đã thực hiện một phần | Đã xuất ảnh dự đoán của 10 ảnh eval |
| Phân tích kết quả | Đã thực hiện | Đã phân tích theo từng lớp và trả lời bốn câu hỏi |

**Tự đánh giá chung:**

Em đã hoàn thành phần kiểm tra dữ liệu, huấn luyện mô hình YOLO26n, đánh giá kết quả trên tập eval và phân tích Gold QC. Nhãn train hiện chưa đạt chuẩn Gold QC (F1 0.5674 so với 0.8) nên mô hình nhận diện kém ở các lớp ít mẫu như bus, van, truck. Phần còn thiếu là sửa nhãn (rework) và chạy lại Gold QC, ghi lại ba quyết định annotation và biên bản self-QC.

---

## 8. Công cụ AI và tài liệu tham khảo

### 8.1. Công cụ đã sử dụng

- **ChatGPT:** Hỗ trợ giải thích các chỉ số trong Object Detection, phân tích Precision, Recall, mAP50, mAP50-95 và hỗ trợ trình bày báo cáo.
- **Claude:** Hỗ trợ phân tích file `annotation_evaluation.json` và hoàn thiện phần Gold QC, trả lời câu hỏi phân tích trong báo cáo.
- **Google Colab:** Sử dụng để chạy notebook, huấn luyện YOLO26n và đánh giá mô hình trên GPU Tesla T4.
- **GitHub:** Sử dụng để lưu trữ repository, dataset, nhãn và kết quả bài làm.
- **Python và Ultralytics:** Sử dụng để kiểm tra dữ liệu, huấn luyện, đánh giá và xuất ảnh dự đoán.

Các kết quả và số liệu trong báo cáo được đối chiếu với output thực tế của notebook và file `annotation_evaluation.json`.

### 8.2. Tài liệu tham khảo

- Notebook thực hành `day2_annotation_to_yolo.ipynb`.
- File hướng dẫn `GUIDELINE_MINI.md`.
- Tài liệu Ultralytics YOLO: https://docs.ultralytics.com/
- Tài liệu PyTorch: https://pytorch.org/docs/stable/index.html
