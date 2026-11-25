# Báo cáo cá nhân — 2A202603236 — Nguyễn Thị Ngọc

## 1. Kết luận trước

Task `D13-2A202603236` gồm 10 frame: `s01f36`, `s01f59`, `s02f10`, `s02f32`, `s03f10`, `s03f41`, `s04f07`, `s04f52`, `s05f03`, `s05f41`. Tôi thao tác trên Windows 11, Chrome và CVAT 2.81.0. Bản xuất Bảng A hiện có 55 cuboid và đã được đối chiếu với phiếu tự kiểm. `s01f36` ít nhãn nhất (4); chưa đủ dữ liệu để kết luận frame này ít vật hay còn bỏ sót vật.

Tôi tự vẽ cuboid trong CVAT. Qua preview, các hộp gần và dễ nhận dạng có vị trí tương đối rõ; điểm cần rà kỹ hơn là đáy, kích thước và hướng ở các hộp chưa thật chắc chắn. Các số liệu dưới đây lấy từ Bảng A và phiếu tự kiểm hiện tại trong repo. Nếu tiếp tục sửa nhãn trên CVAT, cần Save rồi chạy lại `sync` và cập nhật lại các số liệu trong báo cáo.

Khi làm việc, tôi đổi giữa góc nhìn 3D và Top để tìm vật, rồi dùng Top/Side/Front để căn chỉnh hộp. Công cụ có thể xoay và dịch góc nhìn; bật `Cuboid orientation` để kiểm tra hướng. `preview` ghép hộp lên ảnh camera để đối chiếu vật và hướng đầu xe.

## 2. Đọc frame (Bước 2)

Theo hệ tọa độ VF, đầu xe hướng về phía `−y`. Trong BEV của preview, đầu xe ở cạnh trên, `+x` hướng trái và `+y` hướng xuống. Mặt đường tham chiếu xấp xỉ `z = −1,7 m`.

| Frame | Hướng trên BEV | Mặt đường | Vật xa nhất trong Bảng A | Quan sát từ camera và LiDAR |
|---|---|---|---:|---|
| `s01f36` | Đầu xe ở trên; `+x` trái, `+y` dưới | Đường nhìn khá phẳng; dùng mốc gần −1,7 m | Car, 9,99 m | Camera trước thấy xe trên đường; camera bên thấy người và xe hai bánh ở lề. Bảng A hiện có 4 Car; chưa xác nhận các cụm lề đường có đủ điểm để gán. |
| `s01f59` | Đầu xe ở trên; `+x` trái, `+y` dưới | Đường nhìn khá phẳng; đáy hộp có giá trị từ −1,925 đến −1,291 m | Car, 16,44 m | Camera trước và bên thấy nhiều xe, trong đó có xe gần; LiDAR thể hiện vị trí tương đối và khoảng cách 5 hộp đã gán. |

Camera giúp phân biệt màu, hình dáng xe, người/xe đạp, vạch đường và tín hiệu giao thông. LiDAR cho tọa độ 3D theo mét, khoảng cách và hình dạng cụm điểm quanh vật; camera đơn lẻ không đo trực tiếp được chiều sâu chính xác.

| Frame | Hướng trên BEV | Điều đáng chú ý trong preview |
|---|---|---|
| `s02f10` | Đầu trên; `+x` trái, `+y` dưới | Giao lộ đông, có xe buýt và nhiều xe ở các hướng. |
| `s02f32` | Đầu trên; `+x` trái, `+y` dưới | Bốn Car; cuboid `#1` có `length < width`, cần kiểm lại yaw và kích thước. |
| `s03f10` | Đầu trên; `+x` trái, `+y` dưới | Nhiều xe gần nhau; cần đối chiếu kỹ các hộp có vị trí chồng lấn. |
| `s03f41` | Đầu trên; `+x` trái, `+y` dưới | Hộp `#3` ở khoảng 30,34 m có cụm điểm thưa hơn các xe gần. |
| `s04f07` | Đầu trên; `+x` trái, `+y` dưới | Mật độ giao thông cao, gồm Car, Truck và Bus. |
| `s04f52` | Đầu trên; `+x` trái, `+y` dưới | Năm Car trong bản xuất; hộp `#3` có khoảng hở đáy lớn, cần xem lại. |
| `s05f03` | Đầu trên; `+x` trái, `+y` dưới | Có xe gần và một hộp Car kích thước lớn bất thường. |
| `s05f41` | Đầu trên; `+x` trái, `+y` dưới | Camera bên thấy xe buýt gần; Bảng A hiện có 5 Car. |

Ảnh minh chứng T1 của hai frame đọc kỹ là `t1-s01f36-3d.png`, `t1-s01f36-top.png`, `t1-s01f59-3d.png` và `t1-s01f59-top.png` trong gói ảnh nộp LMS.

## 3. Ba cuboid có hướng dẫn (Bước 3)

| Vật | Frame, idx | Class | Hướng và đáy theo số liệu | Kích thước / nhận xét |
|---|---|---|---|---|
| Xe con gần | `s01f59 #5` | Car | `yaw=89,2°`; đáy hộp `z_bottom=-1,925 m` | `4,840 × 1,933 × 1,756 m`; hợp lý với nhóm SUV/minivan hơn sedan nhỏ. |
| Xe buýt | `s02f10 #1` | Bus | `yaw=85,9°`; đáy hộp `z_bottom=-1,892 m` | `12,704 × 3,032 × 3,665 m`; lớn hơn khoảng tham chiếu xe buýt thường, nên đối chiếu hình xe và kích thước hộp. |
| Người đi bộ / xe hai bánh | Chưa có cuboid loại này trong Bảng A | — | Không thể xác nhận hướng hay đáy từ CSV | Preview `s01f36` có người/xe hai bánh ở ảnh camera bên, nhưng hiện chưa có cuboid tương ứng để đánh giá trên LiDAR. |

Ảnh minh chứng T2 gồm `t2-a-car.png`, `t2-b-bus-truck.png` và `t2-c-ped-cyclist.png`. Với người/xe hai bánh, ảnh chỉ minh họa đối tượng quan sát được; không dùng để khẳng định đã gán cuboid khi Bảng A chưa có nhãn tương ứng.

Không có dữ liệu ghi nhận vòng hiệu chuẩn với bạn học hoặc quyết định cụ thể về vật đã bỏ qua. Vì vậy không khẳng định có đối chiếu ngang hàng; các cụm người/xe hai bánh nhìn thấy trong camera cần được xem lại trên point cloud trước khi kết luận có đủ cơ sở gán.

## 4. Bốn hiện tượng chất lượng (Bước 4)

| Hiện tượng | Frame / nguồn | Giả thuyết và quan sát | Ảnh hưởng khi gán |
|---|---|---|---|
| Bóng che | `s03f10`, preview của task cá nhân | Nhiều xe ở gần nhau; xe phía trước có thể che một phần xe phía sau trong góc nhìn. Cần xác nhận vùng mất điểm trực tiếp trên CVAT. | Không coi vùng khuất là đường trống; chỉ gán phần vật có đủ cơ sở suy luận và bật occluded khi phù hợp. |
| Thưa ở xa | `s04f07 #7`, task cá nhân | Car ở `32,16 m`, `68` điểm trong hộp; số điểm thấp hơn rõ so với các xe gần. | Hộp ở xa dễ lệch chiều dài/rộng; chỉ giữ nếu cụm điểm và camera đủ nhận dạng. |
| Phản xạ thấp / ít điểm bất thường | `s03f10 #3`, task cá nhân | Car ở `18,35 m` có `292` điểm; màu tối trên camera có thể là một giả thuyết, nhưng khoảng cách và che khuất cũng ảnh hưởng số điểm. Không có dữ liệu intensity trong bảng này để kết luận nguyên nhân. | Không nới hộp chỉ vì ít điểm; dùng camera và hình dạng LiDAR, ghi nguyên nhân như giả thuyết. |
| Điểm lạ | `s02f35`, ví dụ trong hướng dẫn, task chung `D13-HC485087` | Hướng dẫn mô tả cụm khoảng 46 điểm phía sau-trái, cách khoảng 29 m, ở `z≈−13,5 m`, thấp hơn mặt đường xung quanh. Đây là ví dụ tham khảo, không phải frame trong 10 frame cá nhân. | Không gán cụm đó thành phương tiện; nguyên nhân chưa xác định. |

Ảnh minh chứng T4 lần lượt là `t4-1-bong-che.png`, `t4-2-thua-o-xa.png`, `t4-3-phan-xa-thap.png` và `t4-4-diem-la.png`. Hiện tượng điểm lạ được ghi rõ là ví dụ từ task chung, không tráo thành kết quả của task cá nhân.

## 5. Số đo từ cuboid (Bước 5)

Các số bên dưới lấy từ Bảng A và phiếu tự kiểm hiện có. `r` là khoảng cách không gian `√(x²+y²+z²)`.

| Cuboid | z (m) | h (m) | z đáy (m) | dist_m (m) | r (m) | yaw (°) | points_in | Nhận xét |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| Gần nhất: `s01f59 #5 Car` | −1,047 | 1,756 | −1,925 | 2,96 | 3,14 | 89,2 | 3.238 | Đáy hộp ở `z_bottom=−1,925 m`; đối chiếu thêm với mặt đường trong view Side/Front. |
| Xe cùng chiều tham khảo: `s02f32 #2 Car` | −0,941 | 1,490 | −1,686 | 19,27 | 19,30 | 272,0 | 323 | Yaw gần mốc 270°; cần xác nhận với hướng xe trên camera. |
| Xa nhất: `s04f07 #7 Car` | −0,620 | 1,923 | −1,582 | 32,16 | 32,17 | 92,9 | 68 | Cụm điểm thưa do ở khoảng cách xa; cần đối chiếu camera khi đánh giá hộp. |

Khoảng cách xa nhất là `32,16 m` theo `dist_m` và `32,17 m` theo `r`; gần nhất là `2,96 m` và `3,14 m`. Xe `s01f36 #1` có `yaw=91,6°`, gần mốc 90° của xe đi ngược chiều; hướng cần đối chiếu lại với camera.

| Cuboid Car | dist_m (m) | points_in |
|---|---:|---:|
| `s01f59 #5` | 2,96 | 3.238 |
| `s02f32 #3` | 8,58 | 1.890 |
| `s03f10 #8` | 16,78 | 285 |
| `s04f07 #7` | 32,16 | 68 |

Trong các ví dụ này, khi khoảng cách tăng từ 2,96 m lên 32,16 m (khoảng 10,9 lần), số điểm giảm từ 3.238 xuống 68. Xu hướng phù hợp với mật độ điểm giảm theo khoảng cách, nhưng số điểm còn phụ thuộc kích thước, góc nhìn và che khuất; không thể dùng các hộp khác nhau để suy ra một quy luật nghịch đảo chính xác. Không có lịch sử chỉnh sửa để xác định hai hộp đã sửa sau preview. Cũng chưa có thông tin về lần đổi kiểm với bạn học.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

Nhãn 3D dùng mét vì mô hình cần học vị trí và kích thước vật trong không gian thật, không chỉ kích thước biểu kiến phụ thuộc phối cảnh trên ảnh. Vật ở xa hoặc bị che có ít điểm hơn, nên cần quy tắc riêng để tránh suy diễn quá mức từ cụm điểm thưa.

Tôi trực tiếp đọc point cloud, quan sát camera, vẽ và chỉnh từng cuboid trong CVAT. Tôi cũng tự chạy `sync`, `selfcheck` và `check_submission.py` để đối chiếu số liệu. AI chỉ hỗ trợ tôi hiểu hướng dẫn và cách đọc một số thuật ngữ; AI không tạo nhãn và không quyết định vật nào được gán.

- Công cụ: CVAT, các script `sync`, `selfcheck`, `check_submission.py` và AI hội thoại để hỏi cách hiểu hướng dẫn.
- Câu hỏi chính: cách đọc BEV/yaw/đáy cuboid và các chỉ số kích thước, mặt đường, điểm thưa.
- Cách dùng kết quả: tôi tự kiểm tra lại trên CVAT và point cloud; chỉ dùng AI để tham khảo cách hiểu, không dùng AI để vẽ cuboid, chọn vật hay thay đổi số trong Bảng A.

Những điểm còn cần xác nhận là đáy/kích thước/yaw của một số hộp, các vật hai bánh hoặc người thấy trên camera nhưng chưa có cuboid, và số liệu mới nhất nếu tiếp tục Save trên CVAT. Tại thời điểm chốt báo cáo, `check_submission.py` đã kiểm tra Bảng A và phiếu tự kiểm khớp nhau; nếu có chỉnh sửa mới thì phải chạy lại `sync` trước khi nộp. Ảnh minh chứng được nộp riêng trong ZIP LMS, không commit vào repo public.
