# Các bước hoàn tất báo cáo

## 1. Mở lại CVAT local

Ngày 13/11/2026, trang localhost:8080 báo ERR_CONNECTION_REFUSED. Chưa đủ căn cứ để kết luận container nào đang dừng.

1. Mở Docker Desktop, đợi Docker Engine hoạt động.
2. Mở terminal trong thư mục repository ở VS Code. Chạy `docker ps -a --filter name=cvat`, rồi `docker ps -a --filter name=traefik`.
3. Trong Docker Desktop → Containers, tìm nhóm Compose chứa CVAT và bấm Start cho nhóm. Không xóa container hoặc volume. Nếu container báo lỗi, mở Logs của container đó và gửi đoạn lỗi cuối.
4. Mở http://localhost:8080. Khi giao diện lên, chạy `python lab.py check --no-persist` trong thư mục repository. Tùy chọn này bỏ qua bước ghi Compose từng gây WinError 267; không phải cách tự khởi động Docker Engine.

## 2. Hoàn tất các mục cần kiểm trên ảnh

Mở job người http://localhost:8080/tasks/14/jobs/14 và job mặt http://localhost:8080/tasks/15/jobs/15. Frame dưới đây đánh số từ 0; luôn đối chiếu tên ảnh vì thứ tự có thể thay đổi.

| Job / frame | Ảnh | Cần làm |
|---|---|---|
| 14 / 11 | 000000542160.jpg | Chọn cầu thủ đứng giữa, mở Objects → PARTS, kiểm left_eye/right_eye theo phía của cầu thủ. Cảnh báo của script chưa đủ để kết luận đổi trái/phải. |
| 14 / 4 | 000000157271.jpg | Chọn điểm xanh nằm trên nền cây phía trên-trái đầu người áo xanh 77, đọc tên trong PARTS. Nếu điểm đang outside thì tọa độ cũ không phải lỗi vị trí có hiệu lực; nếu v>0, đặt lại hoặc đổi cờ theo mức nhìn thấy. |
| 14 / 8 | 000000253584.jpg | Phóng to người áo xanh ở hậu cảnh và vùng tối sát mép trái. Đo theo pixel ảnh gốc, không theo kích thước hiển thị sau zoom; chỉ cập nhật số người bỏ qua khi xác định được người và ngưỡng 60 px. |
| 15 / 7 | face_505385.jpg | Kiểm mouth_left và lip_lower; đưa khóe về góc gặp hai môi, môi dưới về giữa mép ngoài môi dưới. |
| 15 / 6 | face_449454.jpg | Kiểm các điểm eye_upper/eye_lower, đặt sát mép mí tại vị trí trên/dưới tâm con ngươi, không đặt ở lông mày. |
| 15 / 5 | face_324513.jpg | Báo cáo đã ghi lý do giữ tám điểm mắt v=0. Hỏi coach cách xử lý khác biệt với quy ước kính râm v=1 của lab; không ghi coach đã đồng ý khi chưa có câu trả lời. |

Sau khi chỉnh, nhấn Ctrl+S và đợi Save hoàn tất. Chỉ cập nhật “Đã sửa” ở các dòng đã thực sự sửa và kiểm lại. Nếu kiểm thấy không có lỗi, đổi mã thành ok kèm lý do; không cần tạo lỗi để điền bảng.

## 3. Xuất và chụp ảnh JSON

1. Trong terminal tại repository, chạy `python lab.py export` sau khi đã Save cả hai job. Đợi xuất và tự kiểm hoàn tất; nếu thất bại, giữ nguyên thông báo lỗi để xử lý.
2. Mở `src/exports/`, chọn ZIP vừa xuất (kiểm ngày sửa), dùng Extract All để giải nén vào thư mục riêng. Mở `annotations/person_keypoints_default.json` bằng VS Code.
3. Nhấn Shift+Alt+F để định dạng JSON; Ctrl+F tìm `num_keypoints`. Chọn một annotation có cả `image_id`, `keypoints`, `num_keypoints`. Có thể bật Word Wrap bằng Alt+Z để mảng không tràn ngang. Không sửa dữ liệu JSON.
4. Nhấn Win+Shift+S, chụp vùng VS Code có tên file và annotation. Mở thông báo Snipping Tool, lưu PNG vào `report/individual/images/4_json_export.png` trong repository.
5. Thay dòng còn thiếu ảnh ở mục 6 bằng `![Đoạn annotation trong JSON xuất](images/4_json_export.png)`.
6. Đối chiếu tên hai ZIP mới và kết quả tự kiểm với mục 5, cập nhật nếu khác. Kiểm lại cảnh báo theo ảnh; kết quả kiểm định dạng ĐẠT không chứng minh mọi tọa độ đều đúng.

## 4. Thông tin thời gian không có nhật ký

Khung 16:00–20:00 do bạn cung cấp đã có trong báo cáo. File ZIP không lưu thời lượng làm từng nhóm, số skeleton tại phút 105 hoặc ảnh từng tốn nhiều thời gian nhất. Nếu có ghi chú hay bản quay màn hình, dùng các mốc đó để tính. Nếu chỉ nhớ gần đúng, ghi rõ “ước lượng hồi tưởng”; nếu không nhớ, giữ “không ghi nhận”. Không dùng thời gian tạo ZIP làm thời gian gán nhãn.
