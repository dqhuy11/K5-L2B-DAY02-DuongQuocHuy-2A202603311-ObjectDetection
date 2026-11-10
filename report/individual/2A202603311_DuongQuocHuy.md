# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Dương Quốc Huy
- MSSV:2A202603311
- Email:duongquochuy1107@gmail.com
- Lớp:L2B
- Link repo bài làm:https://github.com/dqhuy11/K5-L2B-DAY02-DuongQuocHuy-2A202603311-ObjectDetection.git
- Link Colab đã chạy:https://colab.research.google.com/drive/1uTBrc8PV5yRNYBxxKFIF68wMtF1ZZffz#scrollTo=DlysQwtbYz7k

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

Các trường hợp cần chú ý khi gán nhãn:

1. **Truck và van:** Một số xe tải nhỏ có hình dáng tương tự xe van, cần dựa vào đặc điểm phương tiện để phân loại thống nhất.
2. **Car và van:** Một số xe con có thiết kế thân xe dài, dễ nhầm với xe van.
3. **Truck và bus:** Khi phương tiện bị che khuất hoặc nhìn từ góc khó quan sát, việc phân biệt hai lớp có thể gặp khó khăn.

*Lưu ý: Đây là các trường hợp cần kiểm tra và thống nhất theo guideline. Notebook hiện tại chưa lưu lại ba quyết định annotation cụ thể đã được chốt.*

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
  - Không có trường hợp thiếu file nhãn hoặc ảnh tương ứng trong lần kiểm tra này.
- **Danh sách ảnh eval được hiển thị kết quả dự đoán:**
  - `drive_051.jpg`
  - `drive_052.jpg`
  - `drive_053.jpg`
  - `drive_054.jpg`
  - `drive_055.jpg`
  - `drive_056.jpg`
  - `drive_057.jpg`
  - `drive_058.jpg`
  - `drive_059.jpg`
  - `drive_060.jpg`

- **Lỗi bỏ sót/box/class phát hiện khi rà:** Notebook có hiển thị bounding box dự đoán trên 10 ảnh eval, nhưng chưa có ghi nhận cụ thể về lỗi annotation được phát hiện và sửa bằng mắt.
- **Commit khóa nhãn ban đầu:** Chưa xác định được commit khóa nhãn ban đầu. Commit mới nhất được ghi nhận khi chạy notebook là `50eea0e` (Add files via upload).

---

## 4. Gold QC và rework

Dẫn tới file đánh giá: `outputs/annotation_evaluation.json`.

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | Chưa có dữ liệu | Chưa có dữ liệu |
| FP | Chưa có dữ liệu | Chưa có dữ liệu |
| FN | Chưa có dữ liệu | Chưa có dữ liệu |
| Precision | Chưa có dữ liệu | Chưa có dữ liệu |
| Recall | Chưa có dữ liệu | Chưa có dữ liệu |
| F1 | Chưa có dữ liệu | Chưa có dữ liệu |
| mean_iou_of_matches | Chưa có dữ liệu | Chưa có dữ liệu |
| passed | Chưa có dữ liệu | Chưa có dữ liệu |

**Rework:** Chưa có dữ liệu về quá trình Gold QC và các lần sửa nhãn. Vì vậy, chưa thể kết luận có rework hay không.

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
- File kết quả Colab: `outputs/day2_colab_outputs.zip`

Các thư mục kết quả cần được giữ trong repository để có thể kiểm tra lại. Notebook hiện lưu chúng trong `outputs/`, chưa phải `assets/` như cấu trúc được hướng dẫn trong mẫu báo cáo.

### 5.6. Phân tích kết quả

Qua kết quả thực nghiệm, lớp `car` có khả năng nhận diện tốt nhất với Recall đạt 0.636 và mAP50 đạt 0.738.

Ngược lại, lớp `bus` có Recall bằng 0.000, cho thấy mô hình chưa có trường hợp phát hiện đúng nào đạt điều kiện đánh giá. Lớp `van` cũng có kết quả thấp với Recall 0.0833.

Số lượng đối tượng trong tập eval không đồng đều giữa các lớp. Trong đó, car có 94 instances, truck có 5 instances, bus có 2 instances và van có 12 instances. Điều này cho thấy dữ liệu đánh giá còn mất cân bằng giữa các lớp.

### 5.7. Trả lời bốn câu hỏi phân tích

**Câu 1. Lớp nào có recall thấp nhất? Ảnh nào minh họa?**

Lớp `bus` có Recall thấp nhất với giá trị 0.000. Trong tập eval có tổng cộng 2 instances thuộc lớp bus.

Có thể sử dụng ảnh `drive_052.jpg` để kiểm tra trực quan vì kết quả dự đoán của mô hình trên ảnh này có xuất hiện 1 đối tượng bus.

Tuy nhiên, cần đối chiếu bounding box dự đoán với nhãn ground truth để xác định chính xác trường hợp phát hiện đúng hoặc bỏ sót. Việc xuất hiện một box bus trong kết quả dự đoán chưa đủ để kết luận mô hình đã nhận diện đúng đối tượng.

**Câu 2. Nếu mAP50 cao nhưng mAP50-95 thấp, cần kiểm điều gì trên box?**

Điều này cho thấy mô hình có thể nhận diện đúng đối tượng ở ngưỡng IoU 0.5 nhưng độ chính xác về vị trí bounding box chưa tốt khi sử dụng các ngưỡng IoU cao hơn.

Cần kiểm tra:
- Vị trí tâm của bounding box.
- Chiều rộng và chiều cao của box.
- Box có bao sát toàn bộ đối tượng hay không.
- Box có bị lệch, quá rộng hoặc quá nhỏ so với đối tượng thực tế không.

**Câu 3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**

Các lớp cần chú ý là `truck`, `bus` và `van` vì kết quả Recall của các lớp này còn thấp.

- Truck: Recall 0.200.
- Bus: Recall 0.000.
- Van: Recall 0.0833.

Đây là dữ kiện cho thấy mô hình đang gặp khó khăn khi phát hiện các lớp trên. Những trường hợp truck và van, car và van có thể cần được kiểm tra lại theo guideline để đảm bảo cách phân loại thống nhất.

Tuy nhiên, chỉ dựa vào FP và FN hoặc Recall thấp chưa thể kết luận nguyên nhân là do gán nhãn sai. Cần kiểm tra ảnh gốc, nhãn ground truth và bounding box dự đoán trước khi xác định lỗi cụ thể.

**Câu 4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**

Không sử dụng 10 ảnh eval để train vì cần giữ riêng dữ liệu đánh giá nhằm kiểm tra khả năng nhận diện của mô hình trên những ảnh không được dùng trực tiếp để cập nhật trọng số trong quá trình huấn luyện.

Nếu sử dụng những ảnh này để train, kết quả đánh giá có thể không còn khách quan vì mô hình đã được học từ chính dữ liệu đó.

Trong notebook, 10 ảnh eval được khai báo trong `data.yaml` thông qua phần `val`. Chúng được sử dụng để:

- Đánh giá Precision, Recall, mAP50 và mAP50-95 trong quá trình huấn luyện.
- Theo dõi khả năng nhận diện của mô hình qua các epoch.
- Hỗ trợ lựa chọn checkpoint `best.pt`.
- Đánh giá lại mô hình sau khi huấn luyện bằng `best_model.val()`.

**Lưu ý:** 10 ảnh eval trong notebook đồng thời đóng vai trò validation để lựa chọn mô hình, vì vậy chúng không phải một test set độc lập hoàn toàn.

---

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Kết nối repository | Cần đảm bảo notebook sử dụng đúng repository đã chứa dataset và nhãn | Sử dụng Git clone/pull repository | Clone và pull thành công |
| Kiểm tra dataset | Cần kiểm tra số lượng ảnh và file nhãn | Sử dụng Python để đối chiếu tên file ảnh và nhãn | Đạt 50 ảnh train và 10 ảnh eval, các file nhãn tương ứng đầy đủ |
| Huấn luyện YOLO | Cần lựa chọn batch phù hợp với GPU | Sử dụng `batch=-1` để AutoBatch tự xác định | GPU Tesla T4 sử dụng batch size 47 |
| Đánh giá mô hình | Kết quả nhận diện giữa các lớp chưa đồng đều | Sử dụng `best.pt` để đánh giá lại trên tập eval | Đã thu được các chỉ số Precision, Recall và mAP |
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
| Gold QC | Chưa có dữ liệu | Chưa có kết quả annotation_evaluation.json |
| Rework nhãn | Chưa có dữ liệu | Chưa ghi nhận các lần sửa nhãn |
| Huấn luyện YOLO26n | Đã thực hiện | Hoàn thành 50 epochs |
| Đánh giá mô hình | Đã thực hiện | Có đầy đủ Precision, Recall, mAP50 và mAP50-95 |
| Kiểm tra trực quan | Đã thực hiện một phần | Đã xuất ảnh dự đoán của 10 ảnh eval |
| Phân tích kết quả | Đã thực hiện | Đã phân tích kết quả theo từng lớp và bốn câu hỏi cuối notebook |

**Tự đánh giá chung:**

Em đã hoàn thành phần kiểm tra dữ liệu, huấn luyện mô hình YOLO26n và đánh giá kết quả trên tập eval. Tuy nhiên, phần Gold QC, rework và ghi nhận các quyết định annotation vẫn cần được bổ sung để báo cáo có đầy đủ minh chứng theo yêu cầu.

---

## 8. Công cụ AI và tài liệu tham khảo

### 8.1. Công cụ đã sử dụng

- **ChatGPT:** Hỗ trợ giải thích các chỉ số trong Object Detection, phân tích Precision, Recall, mAP50, mAP50-95 và hỗ trợ trình bày báo cáo.
- **Google Colab:** Sử dụng để chạy notebook, huấn luyện YOLO26n và đánh giá mô hình trên GPU Tesla T4.
- **GitHub:** Sử dụng để lưu trữ repository, dataset, nhãn và kết quả bài làm.
- **Python và Ultralytics:** Sử dụng để kiểm tra dữ liệu, huấn luyện, đánh giá và xuất ảnh dự đoán.

Các kết quả và số liệu trong báo cáo được đối chiếu với output thực tế của notebook.

### 8.2. Tài liệu tham khảo

- Notebook thực hành `day2_annotation_to_yolo.ipynb`.
- File hướng dẫn `GUIDELINE_MINI.md`.
- Tài liệu Ultralytics YOLO: https://docs.ultralytics.com/
- Tài liệu PyTorch: https://pytorch.org/docs/stable/index.html
