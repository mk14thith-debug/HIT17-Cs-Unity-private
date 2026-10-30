# Buổi 2 — Bài tập về nhà

[← Lý thuyết Buổi 2](Ly_thuyet.md)

**Nội dung luyện tập:** Điều kiện, vòng lặp, mảng và hàm cơ bản trong C#.

## Yêu cầu chung

- Mỗi bài là một chương trình C# Console riêng.
- Sử dụng kiến thức đã học; không dùng `List`, `Dictionary` hoặc LINQ.
- Giả sử dữ liệu nhập đúng kiểu và nằm trong phạm vi đề bài; chưa cần xử lý nhập sai bằng `TryParse`.
- Mỗi giá trị đầu vào được nhập trên một dòng riêng.
- Khai báo các hàm bên trong class `Program`, bên ngoài `Main`, và sử dụng các hàm đó trong chương trình.
- Đặt tên biến có nghĩa, thụt lề rõ ràng.
- Tự kiểm tra với dữ liệu mẫu và các trường hợp biên.

---

## Bài 1 — Mô phỏng trận chiến

### Đề bài

Một xe tăng có lượng máu ban đầu `initialHp` và lượng giáp cố định `armor`. Xe tăng lần lượt nhận `n` đòn tấn công.

Với mỗi đòn:

```text
Sát thương thực tế = Chỉ số tấn công − Giáp
```

Nếu kết quả nhỏ hơn 0, sát thương thực tế bằng 0.

Sau mỗi đòn, HP giảm theo sát thương thực tế nhưng không được nhỏ hơn 0. Khi HP về 0, xe tăng bị hạ; những đòn còn lại không được xử lý.

### Yêu cầu

1. Nhập và lưu toàn bộ chỉ số tấn công vào mảng `int[]` trước khi mô phỏng.
2. Mô phỏng các đòn theo đúng thứ tự.
3. Với mỗi đòn đã xử lý, in số thứ tự đòn, sát thương thực tế và HP còn lại.
4. Sau mô phỏng, in trạng thái `Song sot` hoặc `Da bi ha`, số đòn đã xử lý và tổng HP thực sự bị mất.
5. Nếu bị hạ, in thêm số thứ tự đòn kết liễu. Số thứ tự đòn bắt đầu từ 1.

**Phân biệt:** Một đòn gây 50 sát thương khi xe tăng chỉ còn 20 HP thì lượng HP thực sự bị mất ở đòn đó là 20.

### Hàm bắt buộc

```csharp
static int CalculateDamage(int attack, int armor)
```

Trả về sát thương thực tế, không âm.

```csharp
static int CalculateRemainingHp(int currentHp, int damage)
```

Trả về HP sau khi nhận sát thương, không thấp hơn 0.

### Đầu vào

Lần lượt gồm:

- `initialHp`: từ 1 đến 1.000.
- `armor`: từ 0 đến 100.
- `n`: từ 1 đến 100.
- `n` chỉ số tấn công: mỗi giá trị từ 0 đến 1.000.

### Ví dụ

**Đầu vào:**

```text
100
10
5
25
5
60
80
20
```

**Đầu ra:**

```text
Don 1: Sat thuong 15 | HP 85
Don 2: Sat thuong 0 | HP 85
Don 3: Sat thuong 50 | HP 35
Don 4: Sat thuong 70 | HP 0
Trang thai: Da bi ha
So don da xu ly: 4
Tong HP da mat: 100
Don ket lieu: 4
```

Đòn thứ 5 không được xử lý vì xe tăng đã bị hạ ở đòn thứ 4.

### Trường hợp tự kiểm tra

- Mọi chỉ số tấn công đều nhỏ hơn hoặc bằng giáp.
- Xe tăng bị hạ ngay ở đòn đầu.
- HP về đúng 0 ở đòn cuối.
- Xe tăng sống sót sau tất cả các đòn; không in dòng đòn kết liễu.

---

## Bài 2 — Thống kê sát thương

### Đề bài

Trong một trận đấu, người chơi thực hiện `n` đòn đánh. Mỗi đòn có một giá trị sát thương:

- Sát thương lớn hơn 0: đánh trúng.
- Sát thương bằng 0: đánh trượt.

Nhập các giá trị sát thương và lưu vào mảng `int[]`, sau đó thống kê kết quả trận đấu.

### Yêu cầu

Tính và in:

1. Tổng sát thương.
2. Số đòn đánh trúng.
3. Số đòn đánh trượt.
4. Sát thương lớn nhất trong một đòn.

### Hàm bắt buộc

```csharp
static int CalculateTotalDamage(int[] damages)
```

Trả về tổng sát thương của các đòn đánh.

Các phần đếm và tìm sát thương lớn nhất có thể thực hiện trong `Main`; không bắt buộc tách thêm hàm.

### Đầu vào

- Dòng đầu là số nguyên `n`, với `1 <= n <= 100`.
- `n` dòng tiếp theo, mỗi dòng là một giá trị sát thương từ 0 đến 1.000.

### Ví dụ

**Đầu vào:**

```text
6
10
0
25
40
0
15
```

**Đầu ra:**

```text
Tong sat thuong: 90
So don danh trung: 4
So don danh truot: 2
Sat thuong lon nhat: 40
```

### Gợi ý

- Dùng vòng lặp để nhập và duyệt mảng.
- Dùng `if–else` để phân loại đánh trúng hoặc đánh trượt.
- Khởi tạo sát thương lớn nhất bằng phần tử đầu tiên của mảng, sau đó so sánh với các phần tử còn lại.

### Trường hợp tự kiểm tra

- Mảng chỉ có một phần tử.
- Tất cả đòn đánh đều trượt.
- Tất cả đòn đánh đều trúng.
- Có nhiều đòn cùng đạt sát thương lớn nhất.

---

## Bài 3 — Tính điểm trận đấu

### Đề bài

Có `n` người chơi tham gia một trận đấu. Với mỗi người, nhập tên, số địch đã hạ `kills` và số lần hỗ trợ `assists`.

Điểm được tính theo công thức:

```text
Điểm = kills × 100 + assists × 30
```

Hãy in điểm của từng người và tìm người có điểm cao nhất.

### Yêu cầu

1. Nhập thông tin của lần lượt `n` người chơi.
2. Tính và in điểm ngay sau khi nhập xong thông tin của mỗi người.
3. Sau khi nhập hết, in tên và điểm của người đứng đầu.
4. Nếu nhiều người cùng đạt điểm cao nhất, chọn người được nhập trước.

**Không yêu cầu lưu danh sách vào mảng hoặc sắp xếp.**

### Hàm bắt buộc

```csharp
static int CalculateScore(int kills, int assists)
```

Trả về điểm của một người chơi theo công thức đề bài.

### Đầu vào

- Dòng đầu là số nguyên `n`, với `1 <= n <= 20`.
- Với mỗi người, nhập lần lượt:
  - Tên không rỗng, có thể chứa khoảng trắng.
  - `kills`: từ 0 đến 100.
  - `assists`: từ 0 đến 100.

### Ví dụ

**Đầu vào:**

```text
3
An
3
1
Binh
2
3
Chi
3
2
```

**Đầu ra:**

```text
An: 330 diem
Binh: 290 diem
Chi: 360 diem
Dan dau: Chi
Diem cao nhat: 360
```

### Gợi ý

- Dùng một biến lưu tên người đứng đầu và một biến lưu điểm cao nhất.
- Có thể khởi tạo điểm cao nhất bằng `-1`, vì điểm hợp lệ luôn không âm.
- Sau mỗi người, so sánh điểm vừa tính với điểm cao nhất.
- Chỉ cập nhật người đứng đầu khi điểm mới **lớn hơn** điểm cao nhất. Nếu bằng nhau, giữ người đã nhập trước.

### Trường hợp tự kiểm tra

- Chỉ có một người chơi.
- Tất cả người chơi đều có điểm 0.
- Hai người cùng đạt điểm cao nhất.
- Người đạt điểm cao nhất được nhập cuối cùng.

---

## Cách nộp bài

Mỗi thư mục bài là một Console project:

```text
Buoi_2/
├── Bai_1/
│   ├── Bai_1.csproj
│   └── Program.cs
├── Bai_2/
│   ├── Bai_2.csproj
│   └── Program.cs
└── Bai_3/
    ├── Bai_3.csproj
    └── Program.cs
```

- Kèm README ngắn ghi cách chạy và các trường hợp đã tự kiểm tra.
- Đẩy mã nguồn và file `.csproj` lên repo cá nhân, không đưa `bin/`, `obj/` lên GitHub.
- Gửi link thư mục `Buoi_2` trên repo cá nhân.

## Tiêu chí đánh giá

| Nội dung | Điểm |
|---|---:|
| Bài 1: Mô phỏng đúng, dừng đúng lúc, tính đúng HP mất và dùng đủ hai hàm | 4 |
| Bài 2: Dùng mảng, thống kê đủ và gọi hàm tính tổng | 2,5 |
| Bài 3: Tính điểm đúng, chọn đúng người đứng đầu và gọi hàm tính điểm | 2,5 |
| Code rõ ràng, có kết quả tự kiểm tra và nộp đủ project | 1 |
| **Tổng** | **10** |
