# Buổi 4 — Bài tập về nhà C#

[← Lý thuyết buổi 4](Ly_thuyet.md) · [Thực hành trên lớp](Ly_thuyet.md#thực-hành-tổng-hợp)

Làm 3 chương trình C# Console, mỗi bài một project. Dùng dữ liệu cố định trong `Main` và in kết quả để đối chiếu với mẫu.

## Bài 1 — Quản lý băng đạn

**Mức độ** Cơ bản. **Kiến thức** Constructor, field, property, đóng gói và `readonly`.

### Yêu cầu

1. Tạo class `Magazine`. Constructor nhận tên vũ khí và sức chứa băng đạn. Giả sử sức chứa hợp lệ từ 1 đến 30 viên. Khi tạo, băng đạn đầy.
2. Dùng field `private` lưu số đạn còn lại và field `private readonly` lưu sức chứa. Cho bên ngoài đọc `Name`, `Capacity` và `Ammo` qua property. Viết `Ammo` bằng getter dạng `=>`.
3. Viết `public void Shoot(int amount)` để trừ số viên đã bắn và `public void Reload(int amount)` để thêm số viên nạp. Bỏ qua lượng nhỏ hơn hoặc bằng 0. Số đạn luôn nằm từ 0 đến sức chứa. Ví dụ, bắn 10 viên khi chỉ còn 6 thì băng hết đạn.

### Chạy thử trong Main

Tạo băng `Armor` có sức chứa 6 và băng `Swift` có sức chứa 4. In số đạn ban đầu của `Armor`. Trên băng đó, lần lượt gọi `Shoot(2)`, `Reload(5)`, `Shoot(10)` và `Reload(-1)`. In số đạn sau mỗi lần gọi, rồi in số đạn của `Swift`.

**Kết quả mong đợi**

```text
Armor: 6/6
Armor: 4/6
Armor: 6/6
Armor: 0/6
Armor: 0/6
Swift: 4/4
```

### Gợi ý

- Xem mục 3 để phân biệt field và property, mục 5 để kiểm soát trạng thái và mục 11 để xác định chỗ gán field `readonly`.
- Hãy tính số đạn mới trước rồi kiểm tra giới hạn theo từng thao tác. Khi bắn, giới hạn dưới là 0. Khi nạp, giới hạn trên là sức chứa.

**Tự kiểm tra.** Bắn khi băng rỗng, nạp khi băng đầy và xác nhận thao tác trên `Armor` chỉ làm thay đổi số đạn của `Armor`.

## Bài 2 — Đội xe thực hiện nhiệm vụ

**Mức độ** Vận dụng. **Kiến thức** Kế thừa, trừu tượng, đa hình, `virtual`, `abstract`, `override` và `base`.

### Yêu cầu

1. Tạo abstract class `MissionTank` có property `Name` chỉ đọc và constructor nhận tên. Khai báo phương thức `abstract int PerformMission()` để mỗi loại xe cung cấp số điểm nhiệm vụ. Viết `virtual string Describe()` trả về `Xe: <ten>`.
2. Tạo hai lớp con và truyền tên cho constructor cha qua `base(name)`.

   | Lớp con | Điểm từ `PerformMission()` | Hậu tố thêm vào `base.Describe()` |
   | --- | ---: | --- |
   | `ScoutTank` | 20 | ` [Trinh sat]` |
   | `RepairTank` | 35 | ` [Sua chua]` |

3. Trong `Main`, tạo `List<MissionTank>` gồm `ScoutTank` tên `Swift` và `RepairTank` tên `Fixer`. Duyệt qua kiểu `MissionTank`, gọi `Describe()` và `PerformMission()` trên từng xe. In điểm từng xe rồi cộng tổng. Để mỗi lớp con quyết định số điểm qua bản `override`.

**Kết quả mong đợi**

```text
Xe: Swift [Trinh sat] | Diem: 20
Xe: Fixer [Sua chua] | Diem: 35
Tong diem: 55
```

### Gợi ý

- Xem mục 6 về `base`, mục 8 về hợp đồng của phương thức và mục 9 về cách gọi qua kiểu cha.
- Gọi `PerformMission()` một lần cho mỗi xe, lưu kết quả vào biến rồi dùng cùng giá trị để in và cộng tổng.

**Tự kiểm tra.** Thêm một `ScoutTank` tên `Echo` vào cuối danh sách và chạy lại với cùng vòng lặp. Tổng phải thành 75. Trong README, giải thích vì sao biến vòng lặp kiểu `MissionTank` có thể gọi đúng hành vi của cả hai loại xe.

**Thử thêm nếu xong sớm.** Tạo class `Scanner` trả về 20 điểm khi quét. Cho `ScoutTank` giữ một field kiểu `Scanner` và gọi thao tác quét trong `PerformMission()`. Giải thích quan hệ **IS-A** giữa `ScoutTank` và `MissionTank`, cùng quan hệ **HAS-A** giữa `ScoutTank` và `Scanner`.

## Bài 3 — Chuẩn bị trận đấu

**Mức độ** Vận dụng. **Kiến thức** `static class`, `const`, `ref`, `out`, `?.` và `??`.

### Yêu cầu

Tạo `static class MatchSetup` có các thành viên sau.

| Thành viên | Hành vi |
| --- | --- |
| `public const int MaxPlayers = 4` | Số người chơi tối đa của một trận. |
| `public static void Swap(ref int first, ref int second)` | Đổi giá trị hai biến của bên gọi. |

Tạo thêm class `PlayerProfile` có `Name` nhận từ constructor. Trong `Main`, tạo biến `PlayerProfile? host = null`. Dùng `?.` và `??` để hiển thị `Guest` khi chưa có chủ phòng. Sau đó gán một profile tên `HIT` và in lại.

### Chạy thử trong Main

1. Thử lần lượt `"2"`, `"4"`, `"0"`, `"5"` và `"abc"`. Gọi `int.TryParse(text, out int count)` ngay trong `Main`. Số người chơi hợp lệ khi đọc được số nguyên và `count` nằm từ 1 đến `MatchSetup.MaxPlayers`. In chuỗi ban đầu, kết quả hợp lệ (`True` hoặc `False`) và giá trị `count` nhận từ `TryParse`.
2. Đặt hai biến vị trí là 1 và 2. Gọi `Swap` rồi in hai giá trị.
3. In tên chủ phòng khi chưa có profile và sau khi tạo profile.

**Kết quả mong đợi**

```text
2: True, 2
4: True, 4
0: False, 0
5: False, 5
abc: False, 0
Sau Swap: 2, 1
Chu phong: Guest
Chu phong: HIT
```

### Gợi ý

- `int.TryParse` trả `true` với chuỗi `"5"` vì đọc được số 5. Khi kiểm tra luật của trận đấu, 5 vượt giới hạn 4 nên kết quả hợp lệ là `False`, còn `count` vẫn là 5. Với `"abc"`, `TryParse` trả `false` và `count` nhận 0.
- Đối chiếu mục 10 và 11 về `static`, `const`. Xem mục 12 về tên dự phòng và mục 13 về `Swap` với `ref` và `int.TryParse` với `out`.

**Tự kiểm tra.** Thử `"1"` và `"-1"`. Kết quả lần lượt là `1: True, 1` và `-1: False, -1`.

## Cách nộp

- Tạo ba project trong `Buoi_4/Bai_1`, `Buoi_4/Bai_2` và `Buoi_4/Bai_3`.
- Mỗi project có mã nguồn, file `.csproj` và README ngắn ghi cách chạy, dữ liệu mẫu và các trường hợp đã kiểm tra.
- Với bài 2, thêm phần giải thích lời gọi đa hình vào README. Bỏ `bin/` và `obj/` khỏi phần nộp.
