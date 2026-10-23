# Buổi 1 — Bài tập về nhà

[← Lý thuyết](Ly_thuyet.md)

**Số bài: 3.** Bài lập trình chỉ cần dùng biến, nhập xuất, toán tử và chuyển đổi kiểu; chưa yêu cầu điều kiện hay vòng lặp. Giả sử đầu vào hợp lệ theo ràng buộc. Mỗi giá trị nhập trên một dòng riêng.

## Bài 1 — Repo học tập đầu tiên

**Mức độ:** Cơ bản. **Kiến thức:** Git và công cụ.

### Yêu cầu

1. Tạo repo GitHub cá nhân dành cho lớp rồi clone về máy.
2. Viết `README.md` gồm họ tên, mục tiêu học C# và Unity, công cụ sẽ dùng. Chỉ đưa thông tin bạn sẵn sàng chia sẻ.
3. Tạo Console project in ra `Hello HIT!` và tên của bạn.
4. Thêm `.gitignore` để bỏ qua `bin/`, `obj/` và dữ liệu cá nhân của IDE.
5. Tạo ít nhất 2 commit có nội dung rõ ràng, push lên GitHub.

### Sản phẩm nộp

Link repo, project chạy được và lịch sử ít nhất 2 commit. Trong README, giải thích ngắn `clone`, `commit`, `push`, `pull` bằng lời của bạn.

### Tự kiểm tra

- Trang repo trên GitHub đã hiển thị mã nguồn và README mới nhất.
- Không có `bin/`, `obj/`, token hay mật khẩu trong commit.
- Sau push, `git status` không còn thay đổi chưa lưu nếu bạn chưa sửa thêm file.

## Bài 2 — Thẻ nhân vật

**Mức độ:** Cơ bản. **Kiến thức:** `string`, `int`, `float`, `bool`, `char`, nhập xuất.

### Đầu vào

Lần lượt nhập tên nhân vật; HP nguyên từ 1 đến 1.000; tốc độ thực từ 0 đến 100; hạng là một chữ cái Latin in hoa. Số thực dùng dấu chấm. Tính `isAlive` từ biểu thức `hp > 0`, không nhập riêng.

### Đầu ra

In thẻ nhân vật gồm tên, HP, tốc độ với 1 chữ số sau dấu thập phân, hạng và `isAlive`. Dấu thập phân khi in có thể theo thiết lập vùng của máy; ví dụ dùng dấu chấm.

### Ví dụ

Đầu vào:

```text
HIT Tank
100
3.5
A
```

Kết quả mong đợi:

```text
Ten: HIT Tank
HP: 100
Toc do: 3.5
Hang: A
Con song: True
```

### Gợi ý và tự kiểm tra

- Có thể đọc hạng bằng `char.Parse(...)`, tốc độ bằng `float.Parse(..., CultureInfo.InvariantCulture)`.
- Định dạng số bằng `speed.ToString("F1")`.
- Thử tên có khoảng trắng, HP bằng 1 và tốc độ bằng 0.
- Không lưu toàn bộ dữ liệu dưới dạng chuỗi: chọn đúng kiểu để còn tính toán ở các buổi sau.

## Bài 3 — Báo cáo trận đấu

**Mức độ:** Vận dụng. **Kiến thức:** `/`, `%`, `const`, ép kiểu.

### Yêu cầu

Nhập số địch đã hạ `enemyCount` và tổng thời gian chơi `totalSeconds`. Mỗi địch được 150 điểm, lưu bằng `const int PointsPerEnemy = 150;`.

Tính tổng điểm, số phút và giây còn lại, số địch hạ trung bình mỗi phút. Công thức trung bình: `enemyCount * 60.0 / totalSeconds`; in 2 chữ số sau dấu thập phân.

### Ràng buộc

- `0 <= enemyCount <= 10000`.
- `1 <= totalSeconds <= 86400`, không có phép chia cho 0.

### Ví dụ

Đầu vào:

```text
12
125
```

Kết quả mong đợi (dấu thập phân có thể theo culture của máy):

```text
Diem: 1800
Thoi gian: 2 phut 5 giay
Dich/phut: 5.76
```

### Tự kiểm tra

| enemyCount | totalSeconds | Điểm | Phút : giây | Địch/phút |
| --- | --- | --- | --- | --- |
| 0 | 60 | 0 | 1 : 0 | 0.00 |
| 1 | 1 | 150 | 0 : 1 | 60.00 |
| 3 | 90 | 450 | 1 : 30 | 2.00 |

## Tiêu chí hoàn thành

- Bài 1: repo và lịch sử đúng yêu cầu.
- Bài 2: dùng đúng các kiểu dữ liệu, đọc đủ thông tin, xuất đúng nội dung.
- Bài 3: tính đúng đơn vị thời gian, không mất phần lẻ do chia nguyên.
- Cả hai bài lập trình đều biên dịch và có kết quả tự kiểm tra trong README bài nộp.
