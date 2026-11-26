# Báo cáo Day 14 — Nguyễn Viết Tiến — 2A202603140

- Mã học viên dùng với `--student`: `140`
- `seed_tag` trong `frames_A.txt`: `0d038e9b72`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: `score_thr` (tăng từ mặc định `0.1` lên khoảng `0.3`–`0.4`), kết hợp với `min_points` (`5` hoặc `10`).
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Trên 3 frame tập A, cấu hình gốc sinh ra tới **94** box trong khi nhãn tham chiếu chỉ có **44** box — lượng **box thừa rất nhiều** (phải xóa **53** box, riêng chi phí xóa đã là `0.3 × 53 = 15.9` trên tổng chi phí `20.4`, tức ~78%), đặc biệt nhiều box `Cyclist` nhiễu (dự đoán 25, thực tế chỉ 4) và các box `Car` ở vùng điểm thưa. Trong khi đó số box bị sót rất ít (chỉ thêm **3** box) và số box lệch cần sửa cũng ít (**3** box). Vì vậy, tăng ngưỡng tin cậy `score_thr` (và lọc thêm bằng `min_points`) sẽ loại bỏ mạnh các box nhiễu điểm tin cậy thấp, giảm chi phí xóa nhiều hơn phần chi phí phát sinh nếu vô tình làm sót thêm một vài vật thể thật.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | 53 | 3 | 3 | 20.4 |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_140/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_140/frames_B.txt --config d14_140/best_config.json --gt d14_140/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
