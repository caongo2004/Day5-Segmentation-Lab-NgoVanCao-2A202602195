# REPORT – Day 5 Segmentation Lab

* Mã học viên theo lớp: **2A202602195**
* Ngày / CVAT local: **17/09/2026**
* Công cụ đã dùng: **Brush / Polygon**

## 1. Bài đã nộp

| Task            | File ZIP đúng tên     | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --------------- | --------------------- | -----------------: | ---------------------------: |
| easy_semantic   | `easy_semantic.zip`   |              3 / 3 |                           20 |
| medium_instance | `medium_instance.zip` |              3 / 3 |                           32 |
| hard_panoptic   | `hard_panoptic.zip`   |              2 / 2 |                           30 |
| cp1_holes       | `cp1_holes.zip`       |              1 / 1 |                            3 |
| cp2_slice       | `cp2_slice.zip`       |              1 / 1 |                            3 |
| cp5_occlusion   | `cp5_occlusion.zip`   |              1 / 1 |                            3 |
| cp3_thin        | `cp3_thin.zip`        |              1 / 1 |                            3 |
| cp4_curb        | `cp4_curb.zip`        |              1 / 1 |                            3 |
| cp6_coverage    | `cp6_coverage.zip`    |              1 / 1 |                            3 |
| **Tổng tối đa** |                       |                    |                      **100** |

## 2. Một quyết định trước khi dùng gợi ý

* Ảnh, vị trí và object Medium đầu tiên tự vẽ: **Một object xe trong ảnh ****`medium_instance`****, nằm ở khu vực phía dưới ảnh.**

* Class và quy tắc tôi dùng để chọn biên: **Tôi gán đúng class của xe và vẽ mask theo phần vật thể thực sự nhìn thấy. Tôi bám sát đường biên bên ngoài của xe, không lấy phần nền và không tự suy đoán phần vật thể bị che khuất. Các object riêng biệt được giữ thành các instance riêng, kể cả khi chúng cùng class.**

* Nếu dùng gợi ý sau đó: **Không dùng gợi ý tự động cho object này. Tôi tự kiểm tra mask bằng cách quan sát đường viền của vật thể, đặc biệt ở những vùng tiếp xúc với nền hoặc bị object khác che.**

* Một quyết định gán nhãn của tôi: **Khi một phần xe bị vật khác che khuất, tôi chỉ gán phần nhìn thấy thay vì tự vẽ tiếp hình dạng phía sau vật che. Nếu có người ngồi trên xe máy và ****`person`**** là một class riêng thì người và xe được coi là hai object riêng, không gộp người vào mask của xe.**

## 3. Một lỗi tôi tìm thấy và sửa

* Task/ảnh/vùng: **`cp5_occlusion`****, vùng object bị che khuất.**

* Lỗi thuộc loại: **biên / phủ vùng**

* Bằng chứng tôi nhìn thấy: **Mask ban đầu có một phần đi vào vùng bị vật khác che hoặc lấy thêm một phần nền không thực sự thuộc object. Khi phóng to ảnh có thể thấy đường biên thật của vật thể kết thúc tại vùng che khuất.**

* Quy tắc và hành động sửa: **Tôi chỉnh lại mask để chỉ giữ phần object thực sự nhìn thấy, xóa phần mask vượt sang nền hoặc phần bị che mà không có đủ bằng chứng để xác định đường biên. Sau đó tôi kiểm tra lại toàn bộ mép mask ở mức zoom lớn.**

* Sau sửa đã Save và export lại chưa? **Đã Save và export lại ****`cp5_occlusion.zip`****.**

**Kết quả scorer/GitHub Actions:** **chưa có**.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí                                                | Hai cách hiểu có thể                                                    | Quy tắc/chứng cứ                                                                                          | Quyết định hoặc câu hỏi cho coach                                                              |
| --------------------------------------------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| 1. `medium_instance`, object xe máy có người ngồi trên xe | Gộp người và xe thành một mask / tách `person` và xe thành hai instance | Người và phương tiện là hai object có class khác nhau và vẫn có đường biên nhận biết được                 | **Tôi tách người và xe thành các object riêng; mask của xe không bao gồm người ngồi trên xe.** |
| 2. `cp5_occlusion`, object bị che bởi object khác         | Đoán và vẽ toàn bộ hình dạng phía sau vật che / chỉ vẽ phần nhìn thấy   | Không có đủ thông tin ảnh để xác định chính xác đường biên của phần bị che                                | **Tôi chỉ vẽ phần nhìn thấy và dừng mask tại ranh giới occlusion.**                            |
| 3. `cp4_curb`, vùng sát mép road và curb                  | Tính vùng đó vào road / tính vào vùng curb hoặc vùng nền nâng cao       | Tôi quan sát thay đổi độ cao, kết cấu và đường ranh giữa mặt đường với bó vỉa thay vì chỉ dựa vào màu sắc | **Tôi đặt đường biên theo mép vật lý của curb; vùng mặt đường phía dưới vẫn thuộc road.**      |
