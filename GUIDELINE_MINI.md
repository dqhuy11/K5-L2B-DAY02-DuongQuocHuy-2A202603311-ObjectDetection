# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Phương tiện giao thông chở người, thường có 4 bánh, kích thước nhỏ hoặc trung bình. |Sedan, SUV, hatchback, taxi, xe con.|
| `truck` |Phương tiện vận tải dùng để chở hàng hóa, thường có kích thước lớn và phần thùng hàng riêng biệt. |Xe tải thùng, xe tải ben, xe đầu kéo. |
| `bus` |Phương tiện giao thông công cộng hoặc chuyên dụng dùng để vận chuyển nhiều hành khách. |Xe buýt, xe khách, xe buýt trường học. |
| `van` |Phương tiện có khoang thân xe kín, dùng để chở người hoặc hàng hóa, thường nhỏ hơn truck và có hình dáng gần giống xe tải nhỏ. |Minivan, cargo van, passenger van. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bounding box phải bao quanh toàn bộ phần vật thể nhìn thấy được, bao gồm thân xe và các bộ phận thuộc về xe. Không lấy quá nhiều vùng nền xung quanh và không cắt mất phần vật thể đang nhìn thấy.
- Vật thể bị che khuất: Nếu xe bị che khuất một phần nhưng vẫn có thể xác định được lớp, tiến hành gán nhãn theo phần vật thể nhìn thấy. Không tự suy đoán phần bị che khuất để mở rộng bounding box.
- Vật thể bị cắt bởi rìa ảnh: Nếu xe chỉ xuất hiện một phần do nằm ngoài khung hình, vẫn gán nhãn nếu có đủ đặc điểm để xác định lớp. Bounding box giới hạn trong phạm vi ảnh, không mở rộng ra ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: Chỉ gán nhãn khi có đủ thông tin hình ảnh để xác định vật thể và phân loại tương đối chắc chắn. Nếu không thể phân biệt giữa các lớp hoặc vật thể quá nhỏ để nhận diện, không tự đoán nhãn và ghi nhận để kiểm tra lại theo guideline chính.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_...` | | | |
| `drive_...` | | | |
| `drive_...` | | | |
