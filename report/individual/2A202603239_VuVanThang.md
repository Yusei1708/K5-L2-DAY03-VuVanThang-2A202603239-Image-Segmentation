# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603239 (Vũ Văn Thắng - K5-L2)
- Ngày / CVAT local: 2026-11-11 / CVAT local (`http://localhost:8080`)
- Công cụ đã dùng: CVAT local (Polygon, Brush) và script hỗ trợ khởi tạo/kiểm tra QC (`scripts/inspect_submissions.py`)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg` (frame 0 của task `medium_instance`), người phụ nữ mặc áo dài đi bộ qua vạch kẻ đường ở chính giữa ảnh.
- Class và quy tắc tôi dùng để chọn biên: Class `person`; bám sát đường viền phần nhìn thấy của đầu, mái tóc, hai tay, tà áo dài phía trước/sau và hai bàn chân tiếp xúc mặt đường; dừng biên chính xác tại ranh giới nơi tà áo dài che ngang thân chiếc xe máy đang chạy phía sau lưng mà không tô tràn sang bánh xe máy hay bóng đổ trên mặt đường.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Trong ảnh `000000181542.jpg`, gợi ý ban đầu nhận diện nhầm hình người mẫu trên tấm áp phích quảng cáo cửa hàng CHANEL ở hậu cảnh thành một object `person` (thừa vật thể) và chiếc xe máy chạy ngang phía sau người phụ nữ mặc áo dài bị tách rời phần đầu xe và đuôi xe. Tôi đã xóa mask `person` bị nhận nhầm trên áp phích CHANEL và gom 2 phần nhìn thấy của chiếc xe máy bị che khuất thành 1 instance `motorcycle` duy nhất.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình: Không áp dụng (đã tự vẽ hoàn chỉnh object đầu tiên rồi mới tham khảo gợi ý cho các object sau như mô tả ở trên).

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng:
  1. `medium_instance` / ảnh `000000181542.jpg` — vùng tấm biển quảng cáo cửa hàng CHANEL ở hậu cảnh phía trên bên trái xe bán tải bạc, và nhóm xe máy ở tiền cảnh.
  2. `easy_semantic` / ảnh `7ee6d192-89e2408b.jpg` — vùng dải phân cách bê tông (jersey barrier) dọc lề trái đường cao tốc.
  3. `cp6_coverage` / ảnh `7daa6479-67988f3f.jpg` — vùng nắp capo/taplo xe gắn camera (ego-vehicle) ở mép dưới ảnh ($y \ge 635$) và dãy xe taxi màu vàng dưới hàng cột tòa nhà bên phải.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: Thừa vật thể (`medium_instance` gán nhầm hình ảnh 2D trên áp phích quảng cáo thành `person`), thiếu lớp do lệch tên nhãn (`motorbike` vs `motorcycle`), sai lớp chức năng (`easy_semantic` gán nhầm dải phân cách bê tông thành `sidewalk`), và tràn/thiếu vùng `car` ở `cp6_coverage` (gán nhầm nắp capo xe gắn camera thành `car` nhưng lại bỏ sót dãy xe taxi vàng phía sau hàng cột bên phải).
- Bằng chứng tôi nhìn thấy: Khi kiểm tra trực quan ảnh overlay từ bản export đầu tiên, trên ảnh `000000181542.jpg` xuất hiện một polygon `person` nằm gọn trong khung kính trưng bày của tiệm CHANEL (tọa độ $x \approx 240\text{–}320, y < 165$), đồng thời các xe máy chưa xuất hiện do lệch tên nhãn với `classes.json`; ở ảnh `7ee6d192-89e2408b.jpg`, dải bê tông chắn làn bên trái đường cao tốc bị phủ màu hồng của lớp `sidewalk`; còn ở ảnh `7daa6479-67988f3f.jpg` (`cp6_coverage`), mép dưới kính chắn gió ($y \ge 635$) bị tô nhầm thành `car` trong khi các xe taxi vàng ở hành lang bên phải bị gộp vào `building`.
- Quy tắc và hành động sửa:
  - Theo quy tắc instance segmentation, ảnh in trên áp phích/biển quảng cáo không phải người thật nên tôi đã xóa các polygon `person` nằm trên poster CHANEL, đồng thời ánh xạ đúng lớp `motorcycle` cho các xe hai bánh có động cơ (tổng số annotation chuẩn của `medium_instance` sau khi sửa là 66 polygons trên 3 ảnh).
  - Theo quy tắc semantic segmentation (`guideline-mini-sheet.md`), phân biệt `road` và `sidewalk` theo chức năng thực tế (vỉa hè cho người đi bộ), không gán dải phân cách bê tông trên cao tốc là `sidewalk`, nên tôi đã xóa mask `sidewalk` sai ở ảnh `7ee6d192-89e2408b.jpg`.
  - Ở `cp6_coverage` (`7daa6479-67988f3f.jpg`), tôi xóa vùng `car` bị nhận nhầm ở taplo/nắp capo xe gắn camera ($y \ge 635$, vốn là vùng `void/ignore` trong BDD100K) và phóng to vùng hành lang bên phải để tách chính xác các xe taxi vàng sang lớp `car` cùng người đi bộ sang lớp `person`.
- Sau sửa đã Save và export lại chưa? Đã Save trên CVAT local, export lại toàn bộ ZIP vào `submissions/` và chạy `python3 scripts/inspect_submissions.py --dir submissions` xác nhận cả 9/9 tasks đạt `[OK]`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Đã kiểm tra cấu trúc tự động qua `inspect_submissions.py` (9/9 tasks `[OK]`, 0 lỗi) / chưa có điểm chấm reference. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `medium_instance` / ảnh `000000181542.jpg` — chiếc xe bán tải (pickup) màu bạc ở trung cảnh phía sau người đi bộ | Cách 1: Gán class `car` vì phần cabin chở người giống SUV/ô tô con. Cách 2: Gán class `truck` vì xe có thùng chở hàng lộ thiên phía sau. | Theo taxonomy chuẩn của COCO (`data/README.md`), dòng xe bán tải (pickup truck) được xếp vào nhóm `truck`, dù ranh giới thị giác với SUV (`car`) khá gần. | Quyết định gán 1 instance `truck` bám sát phần nhìn thấy (trừ phần đầu xe bị người lái xe máy che) và hỏi coach xem bộ reference chấm điểm gán chiếc pickup này là `truck` hay `car`. |
| 2. `cp1_holes` / ảnh `000000144300.jpg` — phần kính chắn gió trong suốt (windshield) và khe hở bánh xe của chiếc mô-tô Honda CB1100R | Cách 1: Khoét rỗng phần kính chắn gió trong suốt vì nhìn xuyên qua thấy chiếc xe caravan phía sau. Cách 2: Giữ nguyên phần kính chắn gió và khe bánh xe nằm bên trong mask của `motorcycle`. | Theo quy tắc của trạm `cp1_holes` trong `CHECKPOINTS.md` và `data/manifest.json`: *"Holes: windows/gaps stay inside the mask — do NOT cut them out"*. | Quyết định giữ toàn bộ phần kính chắn gió và khe bánh xe nằm liền mạch bên trong 1 polygon `motorcycle` duy nhất, không khoét lỗ. |
| 3. `cp6_coverage` / ảnh `7daa6479-67988f3f.jpg` — chiếc xe buýt trắng lớn ở giữa (`MTA New York City Bus 2555`) và xe tải `CAPITAL` bên trái | Cách 1: Để trống (unlabeled/background) vì `classes.json` của `cp6_coverage` chỉ có 7 lớp (`road`, `sidewalk`, `building`, `vegetation`, `sky`, `car`, `person`), không có `bus` hay `truck`. Cách 2: Tô ép thành `car` để tăng tỷ lệ pixel có nhãn. | Theo `CHECKPOINTS.md` và `guideline-mini-sheet.md`: *"Coverage cần đối chiếu class, không tô ép vùng chưa chắc / không bịa class ngoài `classes.json`"*. | Quyết định **không** gán nhầm xe buýt và xe tải sang `car`; chỉ phủ kín toàn bộ các vùng thuộc đúng 7 class trong schema (`road`, `sidewalk`, `building`, `vegetation`, `sky`, `car`, `person`). |
