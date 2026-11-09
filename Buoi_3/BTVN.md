# Buổi 3 — Bài tập về nhà

[← Lý thuyết buổi 3](Ly_thuyet.md) · [Thực hành cuối buổi](Ly_thuyet.md#11-thực-hành-cuối-buổi--hai-bài-tổng-hợp-đơn-giản)

**Số bài** 3 bài C# Console, mỗi bài một project. Giả sử đầu vào đúng định dạng và phạm vi. Nhập mỗi giá trị hoặc lệnh trên một dòng. Dùng các collection đã học và vòng lặp để xử lý dữ liệu.

## Bài 1 — Túi vật phẩm

**Mức độ:** Cơ bản. **Kiến thức:** `List<string>`, `Dictionary<string, int>`.

### Yêu cầu

1. Nhập số lượt nhặt `n` (`0 <= n <= 100`), rồi nhập `n` mã vật phẩm.
2. Nhập một mã cần tra cứu.
3. Nhập một mã cần bỏ khỏi túi.

Mỗi mã là một chuỗi không rỗng, chỉ chứa chữ Latin thường hoặc `_`. Mã cần tra hoặc cần bỏ có thể chưa xuất hiện trong túi.

- Lưu tất cả lượt nhặt vào `List<string>`, giữ nguyên thứ tự và cả mã trùng.
- Tạo Dictionary đếm số lượng của từng mã.
- Dùng `TryGetValue` để lấy số lượng của mã tra cứu trước khi bỏ vật phẩm. Mã chưa có thì số lượng là 0.
- Xóa **lần xuất hiện đầu tiên** của mã cần bỏ trong List và giảm bộ đếm tương ứng. Xóa khóa khi số lượng về 0. Nếu mã cần bỏ vắng mặt, giữ nguyên túi và bộ đếm.

### Đầu ra

1. In `Da nhat: n` với số lượt nhặt ban đầu.
2. In `So luong tra cuu: k` với số lượng của mã tra cứu trước khi bỏ.
3. Nếu không tìm thấy mã cần bỏ, in thêm `Khong co vat pham`.
4. In `Con lai: ` và các mã còn lại theo thứ tự trong List, cách nhau một dấu cách. Nếu túi rỗng, in `Con lai: (rong)`.

### Ví dụ

Đầu vào:

```text
4
ammo
shield
ammo
hp
ammo
ammo
```

Kết quả mong đợi:

```text
Da nhat: 4
So luong tra cuu: 2
Con lai: shield ammo hp
```

### Gợi ý

- `List` trả lời câu hỏi “các món đang nằm theo thứ tự nào?”. `Dictionary` trả lời câu hỏi “mỗi mã có bao nhiêu món?”. Sau khi bỏ một món, cả hai phải mô tả cùng một túi.
- Xem lại giá trị `bool` do `List.Remove` trả về để biết có cần cập nhật bộ đếm không.

### Tự kiểm tra

- `n = 0` cho số lượng tra cứu bằng 0, thông báo không có vật phẩm và túi rỗng.
- Bỏ mã chỉ có một lần làm khóa biến mất khỏi Dictionary.
- Bỏ mã không có giữ nguyên thứ tự và số lượng các món trong túi.

## Bài 2 — Phục vụ theo thứ tự đến

**Mức độ** Cơ bản. **Kiến thức** `Queue<string>`, `Stack<string>` và vòng lặp.

### Yêu cầu

1. Nhập số người `n` (`0 <= n <= 20`), sau đó nhập `n` tên theo thứ tự đến. Mỗi tên nằm trên một dòng. Đưa từng tên vào `Queue<string>`.
2. Lần lượt phục vụ cho đến khi hàng chờ hết người. Mỗi lượt lấy một người ở đầu hàng, in `Phuc vu: <ten>` và đưa tên đó vào `Stack<string>` lịch sử.
3. Sau khi phục vụ xong, xem tên người được phục vụ gần nhất trong Stack và in `Gan nhat: <ten>`. Nếu lịch sử rỗng, in `Chua phuc vu ai`.

### Ví dụ

**Đầu vào**

```text
3
An
Binh
Chi
```

**Kết quả mong đợi**

```text
Phuc vu: An
Phuc vu: Binh
Phuc vu: Chi
Gan nhat: Chi
```

### Gợi ý

- Dùng `Enqueue` khi nhập tên và `Dequeue` khi phục vụ. Hàng chờ có dữ liệu khi `Count > 0`.
- Dùng `Push` để lưu tên đã phục vụ. Khi cần xem người gần nhất, dùng `Peek` sau khi kiểm tra Stack còn phần tử.

### Tự kiểm tra

- `n = 0` chỉ in `Chua phuc vu ai`.
- `n = 1` in đúng một dòng phục vụ, rồi in chính người đó ở dòng gần nhất.

## Bài 3 — Thành tựu và vị trí

**Mức độ:** Vận dụng. **Kiến thức:** HashSet, struct, hành vi sao chép.

### Phần A — Thành tựu

Nhập `n` từ 0 đến 100 rồi `n` mã thành tựu, mỗi mã một dòng. Mã là chuỗi không rỗng, không có khoảng trắng, phân biệt hoa/thường. Dùng `HashSet<string>` để đếm số mã duy nhất. Sau đó nhập một mã tra cứu và in `True`/`False` tùy nó đã xuất hiện hay chưa. Không yêu cầu in thứ tự các phần tử của tập hợp.

Ví dụ đầu vào:

```text
4
first_win
survivor
first_win
collector
survivor
```

Kết quả:

```text
Thanh tuu duy nhat: 3
Da mo khoa: True
```

### Phần B — Sao chép vị trí

Trong cùng project, sau khi in kết quả phần A, thực hiện phần B bằng dữ liệu cố định. Khai báo `struct GridPosition` có hai field `int X` và `int Y`. Tạo biến `original` với `X = 2`, `Y = 3`. Gán `copy = original`, rồi đổi `copy.X = 99`. In hai vị trí theo mẫu bên dưới.

```text
Original: (2, 3)
Copy: (99, 3)
```

### Gợi ý

- Phần A dùng `Count` để lấy số mã khác nhau và `Contains` để kiểm tra mã tra cứu. Thử nhập cùng một mã ba lần hoặc dùng `n = 0`.
- Phần B đặt khai báo `GridPosition` ngoài `Main`. Quan sát giá trị của `original.X` trước và sau khi thay `copy.X`.
- Viết 2–3 câu trong README giải thích kết quả. Thử đổi `struct` thành `class`, giữ hai phép gán và so sánh đầu ra. Khi thử `class`, nhớ tạo đối tượng `original` bằng `new` trước khi gán các field.

## Cách nộp bài

- Tạo ba Console project riêng trong `Buoi_3/Bai_1`, `Buoi_3/Bai_2` và `Buoi_3/Bai_3`.
- Mỗi project có mã nguồn và `.csproj`. Kèm README ngắn ghi cách chạy và các trường hợp đã tự kiểm tra. Với bài 3, thêm phần giải thích kết quả sao chép.
- Đẩy bài lên repo cá nhân và gửi link thư mục `Buoi_3`. Bỏ `bin/` và `obj/` khỏi phần nộp.
