# Danh sách lớp v0 — Đinh Đức Hiếu · G09

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face` (box tài xế) | có / không | Biết đang có tài xế ở ghế lái và khoanh vùng mặt cho các model sau (mắt, hướng nhìn); mất mặt lâu thì cảnh báo camera bị che hoặc tài xế rời vị trí | Có |
| 2 | `eye_left`, `eye_right` | open / closed / unknown, **từng mắt** | Phát hiện buồn ngủ (PERCLOS, mắt nhắm lâu) và chớp mắt; `unknown` tách riêng để model không học nhầm "mắt bị che = mắt nhắm" | Có, trừ khi tròng kính phản xạ LED (G09 D01, D05) |
| 3 | `gaze_zone` | road, mirror_l/r/c, cluster, center, lap, other | Phát hiện mất tập trung: nhìn ngoài `road` quá N giây thì cảnh báo; `lap` gợi ý đang dùng điện thoại | Một phần: hướng đầu thấy rõ, hướng mắt mất khi mắt `unknown` |
| 4 | `glasses` | none / clear / sun / unknown | Chọn cách xử lý mắt (kính cận hay phản xạ, kính râm che mắt); biết khi nào độ tin cậy trạng thái mắt thấp | Một phần: trên NIR kính râm có thể trông trong suốt |
| 5 | `face13` (13 điểm mặt) | điểm + cờ occluded / outside | Đo độ mở mắt (EAR), ngáp (độ mở miệng), ước lượng tư thế đầu (yaw/pitch) | Có, trừ điểm dưới đốm phản xạ hoặc bị tay che |
| 6 | `hand` · `on_wheel` | yes / no / unknown, kèm `side`, `whose` | Kiểm tra tay trên vô lăng (hands-on detection) trước khi trả quyền lái từ chế độ tự lái | Có (hình dạng) |
| 7 | `phone` · `state` | held / mounted / lap | Phát hiện dùng điện thoại khi lái (gọi, nhắn tin); `mounted` là dùng hợp lệ (dẫn đường) | Có nếu thấy cạnh/góc máy; màn hình sáng có thể chói |
| 8 | `person` · `seatbelt` (tài xế) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây, phát hiện thắt sai (dưới nách, sau lưng) | Một phần: dây tối trên áo tối dễ mất |
| 9 | `frame_meta` · `camera_blocked` | yes / no | Phát hiện camera bị che, để hệ thống báo lỗi thay vì cho ra kết quả sai | Có |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` · `seat` | driver, front_passenger, rear_left, rear_middle, rear_right | Đếm người và biết ghế nào có người: nhắc thắt dây theo ghế, điều chỉnh điều hòa/âm thanh theo vùng | Có (hình dạng) |
| 2 | `person` · `seatbelt` | worn_correct / worn_incorrect / not_worn / unknown, theo ghế | Nhắc thắt dây ghế sau (yêu cầu Euro NCAP) | Một phần; SVIRO không có ai thắt dây nên chưa học được `worn_correct` |
| 3 | `child_seat` · `occupied` | no / unknown (v1.0) | Biết ghế trẻ em đang lắp ở ghế nào; nhắc quên trẻ khi khóa xe | Có (hình dạng); bên trong ghế khó đọc |
| 4 | `infant_seat` · `facing` | rear / front | Tắt túi khí ghế trước khi có nôi quay ngược; nhắc quên trẻ trong xe | Có (hình dạng); hướng nôi cần nhìn kỹ tay cầm |
| 5 | `object` · `type` | bag / box / cup / other, theo ghế | Nhắc quên đồ khi rời xe; tách đồ vật khỏi người để không tính nhầm là có người | Có, nhưng mất màu nên nhận bằng hình dạng (G09 S05 cup ~44 px) |
| 6 | `face` (OMS, chỉ box) | có / không | Xác nhận đối tượng là người thật chứ không phải túi/áo; dùng để định vị đầu cho túi khí | Có khi mặt không quá nhỏ |
| 7 | ghế trống (không box, ghi chú) | số ghế trống | Giúp model không báo "có người" khi ghế trống; cân bằng dữ liệu | Có |
| 8 | `frame_meta` · `modality`, `light` | rgb / ir · day / night / unknown | Tách dữ liệu ngày/đêm, RGB/IR khi huấn luyện và đánh giá; IR-sim không thay được IR thật | — |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Khi trong ghế trẻ em có hình giống trẻ em đang ngồi, guideline chỉ cho `occupied = no / unknown` và nói "không gán" trẻ em. Vậy ghế này gán `child_seat` với `occupied = unknown`, hay bỏ hẳn box, hay cần thêm lớp `child` / giá trị `occupied = yes` ở bản 1.x? Chức năng nhắc quên trẻ cần chính trường hợp này.
   Khung ví dụ: G09 S01_rgb / S01_ir (ghế rear_right), S02 (rear_left), S05 (rear_right).
   Bạn tạm gán thế nào và vì sao: Tạm gán `occupied = yes` và báo coach theo GUIDE §3, nên `check_final` còn 6 lỗi. Nếu coach xác nhận thì đổi sang `unknown`, vì guideline bắt chọn `unknown` khi không chắc.
2. Câu hỏi: Khi không thấy mắt (phản xạ kính, mắt `unknown`) nhưng thấy rõ đầu quay về một phía, `gaze_zone` có được suy từ hướng đầu không, hay phải ghi `other`? Guideline nói "theo hướng mắt và đầu" nhưng không nói trường hợp chỉ có một trong hai.
   Khung ví dụ: G09 D05 (kính cận, hai mắt `unknown`, đầu quay về gương trái); bạn review ghi `other`.
   Bạn tạm gán thế nào và vì sao: Tạm gán `mirror_l` vì hướng đầu rõ và hai bạn thống nhất khi review chéo; nếu guideline yêu cầu cả mắt thì sẽ đổi sang `other`.
