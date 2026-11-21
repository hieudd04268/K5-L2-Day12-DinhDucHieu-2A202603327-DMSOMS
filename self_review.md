# Tự đánh giá — Đinh Đức Hiếu · G09

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01_rgb / S01_ir | `child_seat` rear_right | `guess` | Ghi `occupied = yes`, giá trị guideline không cho phép (chỉ `no` / `unknown`). Lỗi lặp lại ở S02 rear_left và S05 rear_right, nên `check_final` còn 6 lỗi | Chưa, đang chờ coach trả lời (câu hỏi leo thang 1) |
| S02_rgb / S02_ir | `person` rear_right | `missing` | OMS người lớn cần `person` kèm box `face` (GUIDE 4.2), nhưng ảnh này chỉ có `person`. Nếu thấy được mặt thì phải thêm box `face`; nếu không thấy thì ghi lý do vào ghi chú | Chưa, cần mở lại trên CVAT để kiểm |
| D01 | `hand` left | `guess` | Ghi `on_wheel = no` vì nghĩ xe tự lái thì tay luôn ở ngoài vô lăng, nhưng tay đang chạm vô lăng | Có, sửa thành `yes` trong `export_final_v2.zip` |
| D03 | `face` + `face13` | `ok` | Hai mắt `closed`, thấy rõ hai mí khép nên đặt đủ 8 điểm mắt; trái/phải theo người (mắt trái y lớn hơn) | Không cần sửa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: `guess`, tức là điền giá trị theo suy đoán hoặc theo ngữ cảnh chứ không theo cái nhìn thấy và guideline (`occupied = yes` ở 3 cảnh S01, S02, S05; `on_wheel = no` ở D01).

Vì sao mình mắc: Mình gán theo điều mình nghĩ là đúng ngoài đời ("có trẻ trong ghế thì là có người", "xe tự lái thì tay không trên vô lăng") rồi mới mở guideline, nên chọn giá trị guideline không cho phép hoặc không khớp với ảnh. Mình chỉ copy shape từ RGB sang IR rồi cũng không đọc lại thuộc tính, nên lỗi lặp ở cả hai ảnh.

Lần sau tránh bằng cách: Trước khi chọn giá trị, đối chiếu đúng danh sách giá trị ở GUIDE 4.1. Gặp ca guideline không nói tới thì chọn `unknown` / `other` và ghi ghi chú rồi hỏi coach, không tự đặt giá trị mới (RULES §4). Chạy `cabin_check` ngay sau 2 cảnh đầu và sửa hết lỗi trước khi gán tiếp.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai: D01 `hand.on_wheel` (mình ghi `no`, Nguyễn Hoài Nam ghi `yes`; đã sửa trong `export_final_v2.zip`, GUIDE 4.7).

Mình học được gì từ dòng đó: `on_wheel` mô tả đúng khung hình này: tay chạm vô lăng thì `yes`, không phụ thuộc xe đang tự lái hay không. Câu "`on_wheel = no` là bình thường" trong GUIDE chỉ nói `no` không phải lỗi, không có nghĩa là mặc định ghi `no`.
