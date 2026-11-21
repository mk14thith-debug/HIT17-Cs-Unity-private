# Buổi 5 — Bài tập về nhà C#

[← Lý thuyết buổi 5](Ly_thuyet.md) · [Thực hành trên lớp](Ly_thuyet.md#thực-hành-cuối-buổi)

Làm 3 chương trình C# Console, mỗi bài một project riêng. Dùng dữ liệu cố định trong `Main`. Mỗi bài có yêu cầu, kết quả mẫu và gợi ý để tự làm.

## Bài 1 — Mục tiêu nhận sát thương

**Mức độ** Cơ bản. **Kiến thức** Interface, đa hình và đóng gói.

### Yêu cầu

1. Khai báo `IGetHit` có phương thức `void GetHit(int damage)`.
2. Tạo class `Tank` và `WoodenCrate` cùng triển khai `IGetHit`. Mỗi class có `Name` và `Hp` đọc được từ bên ngoài, khởi tạo bằng constructor.
3. `GetHit` bỏ qua sát thương nhỏ hơn hoặc bằng 0. Sát thương dương làm giảm HP, thấp nhất là 0. Phương thức chỉ cập nhật HP. In kết quả từ `Main`.
4. Trong `Program`, viết `static void ApplyDamage(IGetHit target, int damage)` để gọi `GetHit` trên mục tiêu nhận vào.

Giả sử tên không rỗng, HP ban đầu từ 1 đến 1.000 và damage từ −1.000 đến 1.000.

### Kịch bản trong Main

Tạo Tank tên `HIT` với 100 HP và WoodenCrate tên `Box` với 30 HP. Gọi `ApplyDamage` để gây 40 sát thương cho từng mục tiêu, rồi in tên và HP còn lại.

**Kết quả mong đợi**

```text
HIT: 60
Box: 0
```

### Gợi ý

- Xem mục 1 của lý thuyết. Phương thức triển khai interface cần có đúng tên, tham số, kiểu trả về và phạm vi `public`.
- `ApplyDamage` biết mục tiêu qua `IGetHit`. Dùng thao tác interface cung cấp để gây sát thương.
- Thử tạo từng class và gọi `GetHit` trước, sau đó chuyển lời gọi qua `ApplyDamage`.

**Tự kiểm tra.** Sát thương 0 hoặc âm giữ nguyên HP. Khi HP đã 0, nhận thêm sát thương vẫn là 0. Trong README, giải thích vì sao cả hai loại mục tiêu truyền được vào cùng hàm `ApplyDamage`.

## Bài 2 — Gây sát thương cho cả đội bằng generic

**Mức độ** Vận dụng nhẹ. **Kiến thức** Generic method, `where` và `List<T>`.

### Yêu cầu

1. Đưa các định nghĩa `IGetHit`, `Tank` và `WoodenCrate` từ bài 1 vào project bài 2.
2. Trong `Program`, viết `static void HitAll<T>(List<T> targets, int damage) where T : IGetHit`. Hàm duyệt các mục tiêu và gọi `GetHit` để gây sát thương cho từng đối tượng.
3. Trong `Main`, tạo `List<Tank>` gồm xe `Alpha` có 100 HP và xe `Beta` có 60 HP. Tạo `List<WoodenCrate>` gồm thùng `Box` có 10 HP.
4. Gọi cùng hàm `HitAll` với mỗi danh sách và sát thương 20. In tên, HP của các đối tượng sau khi xử lý.

Giả sử các danh sách và phần tử đều có giá trị hợp lệ.

**Kết quả mong đợi**

```text
Alpha: 80
Beta: 40
Box: 0
```

### Gợi ý

- Xem mục 3 của lý thuyết. `T` sẽ là `Tank` ở lần gọi với danh sách xe và là `WoodenCrate` ở lần gọi với danh sách thùng.
- Nhìn `where T : IGetHit` để xác định phương thức có thể gọi trên từng phần tử.
- Hai danh sách được tạo riêng. Mỗi lần gọi hàm xử lý một danh sách.

**Tự kiểm tra.** Thêm xe `Gamma` có 50 HP vào đội. Sau cùng đòn 20, Gamma còn 30 HP. Gọi với danh sách rỗng phải chạy bình thường. Trong README, giải thích vì sao hàm gọi được `GetHit` trên phần tử kiểu `T`.

**Làm thêm nếu xong sớm.** Dùng `Where` lọc các xe còn HP lớn hơn 0, sau đó dùng `Select` lấy tên để in. Chạy phần này sau kịch bản mẫu.

## Bài 3 — Bảng điểm phát thông báo

**Mức độ** Vận dụng. **Kiến thức** Event, `Action` và lambda.

### Yêu cầu

1. Tạo class `ScoreBoard` có `Score` ban đầu là 0 và chỉ đọc từ bên ngoài.
2. Khai báo event `public event Action<int>? ScoreChanged`.
3. Viết `public void AddScore(int points)`. Với points dương, cộng vào điểm và phát event một lần, truyền tổng điểm mới. Với points nhỏ hơn hoặc bằng 0, giữ nguyên điểm.
4. Trong `Main`, lưu một lambda in `Diem: <score>` vào biến `Action<int> showScore`. Đăng ký và hủy đăng ký bằng chính biến này.

Giả sử mỗi lần cộng không quá 1.000 điểm và chương trình gọi tối đa 100 lần.

### Kịch bản trong Main

1. Tạo bảng điểm và đăng ký `showScore`.
2. Gọi lần lượt `AddScore(10)`, `AddScore(0)` và `AddScore(20)`.
3. Hủy đăng ký `showScore`, sau đó gọi `AddScore(5)`.
4. In `Diem cuoi: <score>` từ `Main`.

**Kết quả mong đợi**

```text
Diem: 10
Diem: 30
Diem cuoi: 35
```

### Gợi ý

- Xem mục 4 về `Action<int>` và lambda, mục 5 về event.
- Đặt thao tác phát thông báo sau thao tác cập nhật `Score`. Chọn nhánh nào được phép cộng điểm trước khi phát.
- Dùng `?.Invoke` để việc cập nhật điểm hoạt động cả khi chưa có người nghe.
- Dùng `+=` để đăng ký và `-=` để hủy bằng cùng biến delegate.

**Tự kiểm tra.** Thử `AddScore(-5)`, điểm phải giữ nguyên. Tạo bảng mới và cộng 10 điểm trước khi đăng ký, điểm vẫn lên 10. Trong README, giải thích vì sao sau khi hủy `showScore`, điểm cuối vẫn lên 35.

## Cách nộp

- Tạo ba project trong `Buoi_5/Bai_1`, `Buoi_5/Bai_2` và `Buoi_5/Bai_3`.
- Mỗi project có mã nguồn, file `.csproj` và README ngắn ghi cách chạy, kết quả tự kiểm tra và phần giải thích được yêu cầu.
- Gửi link thư mục `Buoi_5` trên repo cá nhân. Bỏ `bin/` và `obj/` khỏi phần nộp.
