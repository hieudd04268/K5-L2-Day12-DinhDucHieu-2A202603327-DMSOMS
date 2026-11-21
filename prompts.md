# Khai báo dùng AI — Đinh Đức Hiếu

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude Code (Claude Opus 5.5) | Cách thêm ảnh vào task/job CVAT đã tạo; có cần tách 22 ảnh cốt lõi và S09–S10 thành 2 task không | Tạo task riêng `Day12-G09-extra` cho S09–S10 | Đối chiếu GUIDE Task 0, SUBMISSION, COMMON_ERRORS |
| 2 | Claude Code | Giải thích guideline: box `face` (đến chân tóc), `seatbelt`, `facing`, khi nào vẽ `face13`, `child_seat` / `infant_seat` | Hiểu quy tắc rồi tự gán | Đọc lại GUIDE §4.1, §4.5–4.8 |
| 3 | Claude Code | Nhờ kiểm job 13, 14 trên CVAT local. **AI đã đọc annotation qua API và chụp màn hình ảnh cabin/tài xế từ CVAT, vi phạm RULES §3.** Tôi không biết trước là AI sẽ chụp màn hình; AI tự báo sau khi đọc RULES | Sửa D04 (thêm `face13`), D06 (`glasses = unknown`) | Chạy `cabin_check`, đọc GUIDE §4.5–4.6 |
| 4 | Claude Code | Chạy `cabin_check` cho export, tạo `check_cpA.txt`, `check_cpB.txt` (chạy bù) | Dùng làm bằng chứng E2, có ghi chú chạy bù | Đọc kết quả trong file |
| 5 | Claude Code | Chạy `cabin_check diff` với bài Nguyễn Hoài Nam; ghi verdict/action vào `disagreement_log.csv` theo kết luận hai bạn đã chốt; điền `TEAM.md` | Dùng làm E3; verdict do hai bạn quyết, AI chỉ ghi lại | Đối chiếu lại từng dòng với bạn review |
| 6 | Claude Code | Tạo yêu cầu export job 13 qua API CVAT; chạy `check_final.txt`, `check_final_v2.txt` bằng script gốc; gom thư mục nộp | Dùng làm E1, E2 | Đọc kết quả trong file, `python -m zipfile -l` |
| 7 | Claude Code | Soạn nháp `ir_log.csv`, `class_list_v0.md`, `self_review.md` từ GUIDE và thuộc tính trong `annotations.xml` của export (AI không xem ảnh, không mở CVAT) | Dùng làm bản nháp E4, E5 và phần tự đánh giá, tự sửa lại cho khớp với điều mình thấy trên ảnh | Mở lại từng khung trên CVAT để đối chiếu |