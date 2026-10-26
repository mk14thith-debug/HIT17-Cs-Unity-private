# Buổi 2 — Điều kiện, vòng lặp, mảng, chuỗi và hàm trong C#

## 1. Mục tiêu buổi học

Sau buổi học, học viên có thể:

- Sử dụng điều kiện để lựa chọn hành động.
- Sử dụng vòng lặp để thực hiện công việc nhiều lần.
- Lưu và xử lý nhiều phần tử bằng mảng.
- Thực hiện các thao tác cơ bản với chuỗi.
- Viết hàm có tham số, có hoặc không có giá trị trả về.
- Hiểu overload và viết code theo quy ước C# cơ bản.

**Kiến thức cần có:** Biến, kiểu dữ liệu, nhập xuất, toán tử và chuyển đổi kiểu ở Buổi 1.

Các đoạn code ngắn trong tài liệu được đặt bên trong `Main`, trừ khi có hướng dẫn khác. Những ví dụ ghi “chương trình đầy đủ” có thể thay toàn bộ nội dung `Program.cs` để chạy.

---

## 2. Cấu trúc điều kiện

Chương trình thường cần lựa chọn hành động dựa trên dữ liệu. Ví dụ:

- HP lớn hơn 0 thì nhân vật còn sống.
- Có đủ tiền thì được mua vật phẩm.
- Người dùng chọn mục nào thì chương trình thực hiện chức năng đó.

Trong C#, điều kiện của `if` phải có kiểu `bool`: `true` hoặc `false`.

### 2.1. Câu lệnh if

**Cú pháp:**

```csharp
if (dieuKien)
{
    // Các câu lệnh chạy khi điều kiện đúng.
}
```

**Ví dụ:**

```csharp
int hp = 20;

if (hp > 0)
{
    Console.WriteLine("Nhan vat con song.");
}
```

Nếu điều kiện sai, chương trình bỏ qua khối lệnh của `if`.

### 2.2. Câu lệnh if–else

Dùng khi cần xử lý hai trường hợp.

```csharp
int hp = 0;

if (hp > 0)
{
    Console.WriteLine("Nhan vat con song.");
}
else
{
    Console.WriteLine("Nhan vat da bi ha.");
}
```

Chỉ một trong hai khối lệnh được thực hiện.

### 2.3. Câu lệnh if–else if–else

Dùng khi có nhiều trường hợp cần xét.

```csharp
int hp = 25;

if (hp <= 0)
{
    Console.WriteLine("Nhan vat da bi ha.");
}
else if (hp <= 30)
{
    Console.WriteLine("Can hoi mau.");
}
else
{
    Console.WriteLine("San sang chien dau.");
}
```

Chương trình kiểm tra từ trên xuống. Khi gặp điều kiện đúng, nó thực hiện khối lệnh tương ứng và bỏ qua những nhánh còn lại.

**Lưu ý về thứ tự điều kiện:**

```csharp
int hp = 0;

if (hp <= 30)
{
    Console.WriteLine("Mau thap.");
}
else if (hp <= 0)
{
    Console.WriteLine("Da bi ha.");
}
```

Trong ví dụ này, `hp = 0` đã thỏa điều kiện đầu tiên nên nhánh `Da bi ha` không được chạy. Vì vậy, cần sắp xếp điều kiện phù hợp.

### 2.4. Kết hợp điều kiện

| Toán tử | Ý nghĩa |
|---|---|
| `&&` | Cả hai điều kiện cùng đúng |
| `\|\|` | Có ít nhất một điều kiện đúng |
| `!` | Đảo giá trị đúng/sai |

```csharp
int hp = 100;
int ammo = 5;
bool isPaused = false;

if (hp > 0 && ammo > 0 && !isPaused)
{
    Console.WriteLine("Co the ban.");
}
```

Ví dụ dùng `||`:

```csharp
bool hasKey = false;
bool isAdmin = true;

if (hasKey || isAdmin)
{
    Console.WriteLine("Duoc mo cua.");
}
```

### 2.5. Câu lệnh switch–case

`switch` phù hợp khi cần xử lý theo các giá trị cụ thể, chẳng hạn lựa chọn trong menu.

```csharp
int choice = 2;

switch (choice)
{
    case 1:
        Console.WriteLine("Bat dau tro choi.");
        break;

    case 2:
        Console.WriteLine("Xem huong dan.");
        break;

    case 0:
        Console.WriteLine("Thoat.");
        break;

    default:
        Console.WriteLine("Lua chon khong hop le.");
        break;
}
```

- `case`: Trường hợp cần xử lý.
- `break`: Kết thúc phần xử lý của nhánh và thoát khỏi `switch`.
- `default`: Xử lý khi không khớp các `case` đã khai báo.

### 2.6. Toán tử ba ngôi `? :`

Dùng để chọn một trong hai giá trị theo điều kiện.

**Cú pháp:**

```csharp
dieuKien ? giaTriKhiDung : giaTriKhiSai
```

**Ví dụ:**

```csharp
int hp = 50;

string status = hp > 0 ? "Alive" : "Dead";

Console.WriteLine(status);
```

Có thể viết tương đương bằng `if–else`:

```csharp
string status;

if (hp > 0)
{
    status = "Alive";
}
else
{
    status = "Dead";
}
```

Ưu tiên dùng toán tử ba ngôi cho lựa chọn ngắn. Nếu có nhiều thao tác hoặc nhiều nhánh, dùng `if–else` sẽ dễ đọc hơn.

---

## 3. Các cấu trúc vòng lặp

Vòng lặp giúp thực hiện một công việc nhiều lần mà không phải viết lặp lại cùng một đoạn code.

### 3.1. Vòng lặp for

Thường dùng khi biết số lần lặp hoặc cần xử lý theo chỉ số.

**Cú pháp:**

```csharp
for (khoiTao; dieuKien; capNhat)
{
    // Công việc cần lặp.
}
```

**Ví dụ:**

```csharp
for (int i = 0; i < 5; i++)
{
    Console.WriteLine(i);
}
```

**Kết quả:**

```text
0
1
2
3
4
```

**Thứ tự thực hiện:**

1. Khởi tạo `i = 0` một lần.
2. Kiểm tra `i < 5`.
3. Nếu đúng, chạy khối lệnh.
4. Thực hiện `i++`.
5. Quay lại kiểm tra điều kiện.

**Ví dụ tính tổng từ 1 đến n:**

```csharp
int n = 5;
int sum = 0;

for (int i = 1; i <= n; i++)
{
    sum += i;
}

Console.WriteLine(sum); // 15
```

### 3.2. Vòng lặp while

Kiểm tra điều kiện trước mỗi lượt lặp.

```csharp
int ammo = 3;

while (ammo > 0)
{
    Console.WriteLine("Ban!");
    ammo--;
}

Console.WriteLine("Het dan.");
```

Nếu điều kiện sai ngay từ đầu, khối lệnh không chạy lần nào.

**Lỗi thường gặp:**

```csharp
int ammo = 3;

while (ammo > 0)
{
    Console.WriteLine("Ban!");
}
```

Vì `ammo` không thay đổi, điều kiện luôn đúng và vòng lặp tiếp tục chạy. Cần bảo đảm chương trình có cách cập nhật điều kiện hoặc thoát vòng lặp.

### 3.3. Vòng lặp do–while

Thực hiện khối lệnh trước, sau đó mới kiểm tra điều kiện. Vì vậy, khối lệnh luôn chạy ít nhất một lần.

```csharp
int count = 0;

do
{
    Console.WriteLine("Khoi lenh duoc chay.");
    count++;
}
while (count < 0);
```

Dù điều kiện `count < 0` sai, dòng thông báo vẫn được in một lần.

**Lưu ý:** Có dấu `;` sau `while (...)` trong cấu trúc `do–while`.

### 3.4. Vòng lặp foreach

Dùng để duyệt từng phần tử trong một tập dữ liệu, chẳng hạn mảng.

```csharp
string[] playerNames = { "An", "Binh", "Chi" };

foreach (string playerName in playerNames)
{
    Console.WriteLine(playerName);
}
```

Mỗi lượt lặp, `playerName` nhận phần tử tiếp theo.

`foreach` phù hợp khi chỉ cần đọc từng phần tử và không cần chỉ số. Không thể gán lại trực tiếp biến lặp như sau:

```csharp
foreach (string playerName in playerNames)
{
    // playerName = "HIT"; // Lỗi biên dịch.
}
```

### 3.5. break và continue

**`break`: Thoát khỏi vòng lặp gần nhất.**

```csharp
for (int i = 1; i <= 5; i++)
{
    if (i == 3)
    {
        break;
    }

    Console.WriteLine(i);
}
```

Kết quả:

```text
1
2
```

**`continue`: Bỏ phần còn lại của lượt hiện tại và chuyển sang lượt tiếp theo.**

```csharp
for (int i = 1; i <= 5; i++)
{
    if (i == 3)
    {
        continue;
    }

    Console.WriteLine(i);
}
```

Kết quả:

```text
1
2
4
5
```

Với vòng `for`, sau `continue`, phần cập nhật như `i++` vẫn được thực hiện trước khi kiểm tra điều kiện tiếp theo.

### 3.6. Chọn vòng lặp phù hợp

| Tình huống | Cấu trúc thường dùng |
|---|---|
| Lặp 10 lần | `for` |
| Lặp khi vẫn còn đạn | `while` |
| Hiển thị menu ít nhất một lần | `do–while` |
| Đọc lần lượt các tên trong mảng | `foreach` |

---

## 4. Quy ước trình bày code C#

Quy ước giúp code dễ đọc, dễ sửa và thống nhất khi làm việc nhóm.

### 4.1. Đặt tên

| Thành phần | Quy ước | Ví dụ |
|---|---|---|
| Biến cục bộ | `camelCase` | `currentHp`, `playerName` |
| Tham số | `camelCase` | `damage`, `maxHp` |
| Class | `PascalCase` | `Player`, `Tank` |
| Phương thức | `PascalCase` | `CalculateDamage`, `PrintMenu` |
| Property — sẽ học ở buổi sau | `PascalCase` | `Hp`, `Name` |

Tên nên thể hiện ý nghĩa:

```csharp
int enemyCount = 10;
int totalScore = 1500;
```

Biến `i`, `j` phù hợp cho chỉ số vòng lặp ngắn. Với dữ liệu có vai trò rõ ràng, nên dùng tên mô tả vai trò đó.

### 4.2. Thụt lề và dấu ngoặc

Thống nhất thụt lề 4 dấu cách và đặt ngoặc khối trên dòng riêng:

```csharp
if (hp > 0)
{
    Console.WriteLine("Alive");
}
```

Nên dùng `{ }` kể cả khi nhánh chỉ có một câu lệnh, giúp tránh nhầm lẫn khi thêm code sau này.

### 4.3. Tổ chức nội dung

- Mỗi câu lệnh nên nằm trên một dòng.
- Dùng khoảng trắng quanh các toán tử để dễ đọc.
- Tách các công việc độc lập thành hàm.
- Chú thích để giải thích ý định hoặc lý do cần thiết.

---

## 5. Mảng

### 5.1. Mảng là gì?

Mảng lưu nhiều phần tử cùng kiểu dữ liệu trong một biến.

Ví dụ, thay vì khai báo riêng:

```csharp
int damage1 = 10;
int damage2 = 20;
int damage3 = 15;
```

Có thể dùng:

```csharp
int[] damages = { 10, 20, 15 };
```

Độ dài của một mảng được cố định sau khi tạo.

### 5.2. Khai báo và khởi tạo

```csharp
int[] scores = new int[5];
string[] playerNames = new string[3];

int[] damages = { 10, 20, 15 };
string[] items = { "Ammo", "Shield", "Potion" };
```

- `new int[5]`: Tạo mảng 5 số nguyên, ban đầu đều bằng 0.
- `new string[3]`: Tạo mảng 3 chuỗi, ban đầu các phần tử đều là `null`.

Cần gán giá trị chuỗi trước khi sử dụng các thành viên như `.Length` trên phần tử đó.

### 5.3. Truy cập phần tử

Chỉ số mảng bắt đầu từ **0**.

```csharp
int[] damages = { 10, 20, 15 };

Console.WriteLine(damages[0]); // 10
Console.WriteLine(damages[1]); // 20
Console.WriteLine(damages[2]); // 15

damages[1] = 50;

Console.WriteLine(damages[1]); // 50
```

Với mảng có `n` phần tử, chỉ số hợp lệ từ `0` đến `n - 1`.

### 5.4. Length và duyệt mảng

```csharp
int[] damages = { 10, 20, 15 };

Console.WriteLine(damages.Length); // 3

for (int i = 0; i < damages.Length; i++)
{
    Console.WriteLine($"Don {i + 1}: {damages[i]}");
}
```

Không dùng:

```csharp
// Sai: i có thể bằng damages.Length.
// for (int i = 0; i <= damages.Length; i++)
```

Khi truy cập phần tử có chỉ số bằng `Length`, chương trình sẽ gặp lỗi vượt phạm vi mảng.

### 5.5. Tính tổng và trung bình

```csharp
int[] damages = { 10, 20, 15 };

int total = 0;

foreach (int damage in damages)
{
    total += damage;
}

double average = (double)total / damages.Length;

Console.WriteLine($"Tong: {total}");
Console.WriteLine($"Trung binh: {average:F2}");
```

Ở đây mảng có ít nhất một phần tử. Nếu mảng có thể rỗng, cần kiểm tra trước khi chia cho `Length`.

### 5.6. Tìm giá trị lớn nhất

```csharp
int[] damages = { 10, 20, 15 };

int maxDamage = damages[0];

for (int i = 1; i < damages.Length; i++)
{
    if (damages[i] > maxDamage)
    {
        maxDamage = damages[i];
    }
}

Console.WriteLine(maxDamage); // 20
```

Khởi tạo bằng phần tử đầu tiên khi biết mảng không rỗng. Không mặc định dùng 0, vì dữ liệu trong một bài toán khác có thể toàn số âm.

---

## 6. Chuỗi — string

### 6.1. Nối chuỗi và string interpolation

Có thể nối chuỗi bằng `+`:

```csharp
string playerName = "HIT";
int hp = 100;

Console.WriteLine("Player: " + playerName + " | HP: " + hp);
```

Hoặc dùng string interpolation với ký hiệu `$`:

```csharp
Console.WriteLine($"Player: {playerName} | HP: {hp}");
```

Biểu thức bên trong `{ }` được tính rồi chèn vào chuỗi.

```csharp
int attack = 20;
int bonus = 5;

Console.WriteLine($"Sat thuong: {attack + bonus}");
```

### 6.2. Length

Lấy độ dài chuỗi:

```csharp
string name = "HIT Tank";

Console.WriteLine(name.Length); // 8
```

Với dữ liệu thông thường trong bài tập, có thể dùng để đếm độ dài tên. Chính xác hơn, `Length` đếm đơn vị mã UTF-16 nên một số emoji hoặc ký tự kết hợp không tương ứng một-một với ký tự hiển thị.

### 6.3. Substring

Lấy một phần chuỗi.

```csharp
string name = "Tank Alpha";

string firstPart = name.Substring(0, 4);
string secondPart = name.Substring(5);

Console.WriteLine(firstPart);  // Tank
Console.WriteLine(secondPart); // Alpha
```

- `Substring(startIndex, length)`: Lấy `length` đơn vị, bắt đầu từ `startIndex`.
- `Substring(startIndex)`: Lấy từ vị trí chỉ định đến cuối chuỗi.

Phải bảo đảm chỉ số và độ dài hợp lệ.

### 6.4. Contains

Kiểm tra chuỗi có chứa một chuỗi con hay không.

```csharp
string name = "Tank Alpha";

Console.WriteLine(name.Contains("Tank")); // True
Console.WriteLine(name.Contains("tank")); // False
```

Cách gọi trên phân biệt hoa/thường.

Một cách tìm không phân biệt hoa/thường, phù hợp cho ví dụ tên Latin của buổi này:

```csharp
string keyword = "tank";

bool isFound = name.ToLower().Contains(keyword.ToLower());

Console.WriteLine(isFound); // True
```

### 6.5. Replace

Trả về chuỗi mới sau khi thay thế nội dung.

```csharp
string itemName = "Basic Gun";
string upgradedName = itemName.Replace("Basic", "Heavy");

Console.WriteLine(upgradedName); // Heavy Gun
Console.WriteLine(itemName);     // Basic Gun
```

### 6.6. ToUpper và ToLower

```csharp
string name = "Hit Tank";

Console.WriteLine(name.ToUpper()); // HIT TANK
Console.WriteLine(name.ToLower()); // hit tank
```

### 6.7. Chuỗi là bất biến

Các phương thức xử lý chuỗi ở trên không sửa nội dung của chuỗi ban đầu. Chúng trả về kết quả mới.

```csharp
string name = "hit";

name.ToUpper();

Console.WriteLine(name); // hit
```

Muốn biến `name` giữ kết quả mới, cần gán lại:

```csharp
name = name.ToUpper();

Console.WriteLine(name); // HIT
```

---

## 7. Hàm — phương thức

### 7.1. Hàm dùng để làm gì?

Hàm giúp đóng gói một công việc để gọi lại nhiều lần.

Ví dụ:

- In menu.
- Tính sát thương.
- Kiểm tra số chẵn.
- Tính tổng mảng.

Trong class C#, các hàm như trong ví dụ dưới đây được gọi là **phương thức**.

### 7.2. Cú pháp cơ bản

Trong các Console project của buổi này, đặt các hàm bên trong `Program` và ngoài `Main`.

```csharp
static KieuTraVe TenHam(DanhSachThamSo)
{
    // Xử lý.
}
```

Tạm thời dùng `static` để gọi trực tiếp từ `Main`. Ý nghĩa chi tiết của `static` sẽ học ở Buổi 4.

### 7.3. Hàm không trả về giá trị

Dùng kiểu trả về `void`.

**Chương trình đầy đủ:**

```csharp
using System;

class Program
{
    static void PrintMenu()
    {
        Console.WriteLine("1. Bat dau");
        Console.WriteLine("2. Huong dan");
        Console.WriteLine("0. Thoat");
    }

    static void Main()
    {
        PrintMenu();
    }
}
```

Khi gọi `PrintMenu()`, chương trình chạy các câu lệnh trong hàm rồi quay lại vị trí gọi.

### 7.4. Hàm có tham số

Tham số giúp hàm nhận dữ liệu cần xử lý.

```csharp
static void PrintPlayer(string name, int hp)
{
    Console.WriteLine($"{name}: {hp} HP");
}
```

Gọi trong `Main`:

```csharp
PrintPlayer("HIT", 100);
PrintPlayer("Enemy", 50);
```

- `name`, `hp` trong khai báo là **tham số**.
- `"HIT"`, `100` trong lời gọi là **đối số**.

### 7.5. Hàm có giá trị trả về

```csharp
static int CalculateDamage(int attack, int armor)
{
    int damage = attack - armor;

    if (damage < 0)
    {
        return 0;
    }

    return damage;
}
```

Gọi trong `Main`:

```csharp
int result = CalculateDamage(30, 12);

Console.WriteLine(result); // 18
```

`return` kết thúc lần gọi hàm hiện tại và trả giá trị về nơi gọi.

Với hàm trả về `int`, mọi đường chạy có thể đi đến cuối hàm phải có giá trị trả về phù hợp.

### 7.6. Hàm trả về bool

```csharp
static bool IsEven(int number)
{
    return number % 2 == 0;
}
```

Sử dụng:

```csharp
if (IsEven(10))
{
    Console.WriteLine("So chan.");
}
```

Tên như `IsEven`, `IsAlive`, `HasAmmo` giúp người đọc nhận ra hàm đang kiểm tra một điều kiện.

### 7.7. Phạm vi biến

Biến cục bộ chỉ được sử dụng trong phạm vi khai báo của nó.

```csharp
if (true)
{
    int bonus = 10;
    Console.WriteLine(bonus);
}

// Console.WriteLine(bonus); // Lỗi: bonus không tồn tại ở đây.
```

Biến trong một hàm không tự dùng được trong hàm khác. Khi cần, truyền dữ liệu qua tham số hoặc trả kết quả bằng `return`.

### 7.8. Overload — nạp chồng phương thức

Cho phép nhiều phương thức cùng tên nhưng khác danh sách tham số.

```csharp
static int Add(int a, int b)
{
    return a + b;
}

static double Add(double a, double b)
{
    return a + b;
}

static int Add(int a, int b, int c)
{
    return a + b + c;
}
```

Gọi trong `Main`:

```csharp
Console.WriteLine(Add(2, 3));       // 5
Console.WriteLine(Add(2.5, 3.5));   // 6
Console.WriteLine(Add(1, 2, 3));    // 6
```

Overload có thể khác về:

- Số lượng tham số.
- Kiểu tham số.
- Thứ tự các kiểu tham số.

**Chỉ thay tên tham số hoặc kiểu trả về thì không tạo overload hợp lệ.**

Ví dụ không hợp lệ:

```csharp
// Hai khai báo dưới đây không được cùng tồn tại:
//
// static int Calculate(int value) { ... }
// static double Calculate(int value) { ... }
```

---

## 8. Kiểm tra dữ liệu nhập bằng TryParse

Ở Buổi 1, `int.Parse` được dùng với giả định đầu vào hợp lệ. Nếu người dùng nhập `abc`, việc chuyển sang số nguyên sẽ gây lỗi.

`int.TryParse` cho phép kiểm tra việc chuyển đổi có thành công hay không:

```csharp
Console.Write("Nhap HP: ");
string input = Console.ReadLine() ?? "";

if (int.TryParse(input, out int hp))
{
    Console.WriteLine($"HP vua nhap: {hp}");
}
else
{
    Console.WriteLine("Du lieu khong phai so nguyen hop le.");
}
```

- Giá trị trả về là `true` nếu chuyển đổi thành công.
- Biến `hp` nhận kết quả qua `out`.
- Khi chuyển đổi thất bại, `hp` nhận 0.
- Trong buổi này, học cách sử dụng mẫu trên; tham số `out` sẽ được giải thích kỹ ở Buổi 4.

Có thể kiểm tra thêm phạm vi:

```csharp
if (int.TryParse(input, out int hp) && hp >= 0 && hp <= 100)
{
    Console.WriteLine($"HP hop le: {hp}");
}
else
{
    Console.WriteLine("Hay nhap so nguyen tu 0 den 100.");
}
```

**Lưu ý:** Nếu dùng các ví dụ này trong cùng một phạm vi, tránh khai báo lại nhiều biến cùng tên `hp`.

---

## 9. Ví dụ tổng hợp — Menu kiểm tra HP

Ví dụ này kết hợp:

- Hàm.
- `do–while`.
- `switch`.
- Điều kiện.
- `TryParse`.
- String interpolation.

**Chương trình đầy đủ:**

```csharp
using System;

class Program
{
    static void PrintMenu()
    {
        Console.WriteLine();
        Console.WriteLine("1. Kiem tra trang thai nhan vat");
        Console.WriteLine("0. Thoat");
        Console.Write("Lua chon: ");
    }

    static string GetStatus(int hp)
    {
        if (hp == 0)
        {
            return "Da bi ha";
        }

        if (hp <= 30)
        {
            return "Can hoi mau";
        }

        return "San sang";
    }

    static void Main()
    {
        int choice = -1;

        do
        {
            PrintMenu();

            string? input = Console.ReadLine();

            if (input == null)
            {
                break;
            }

            if (!int.TryParse(input, out choice))
            {
                choice = -1;
                Console.WriteLine("Lua chon khong hop le.");
                continue;
            }

            switch (choice)
            {
                case 1:
                {
                    Console.Write("Nhap HP tu 0 den 100: ");
                    string hpInput = Console.ReadLine() ?? "";

                    if (int.TryParse(hpInput, out int hp)
                        && hp >= 0
                        && hp <= 100)
                    {
                        Console.WriteLine($"Trang thai: {GetStatus(hp)}");
                    }
                    else
                    {
                        Console.WriteLine("HP khong hop le.");
                    }

                    break;
                }

                case 0:
                    Console.WriteLine("Tam biet!");
                    break;

                default:
                    Console.WriteLine("Lua chon khong hop le.");
                    break;
            }
        }
        while (choice != 0);
    }
}
```

**Giải thích thêm:**

- `string?` biểu thị biến chuỗi có thể nhận `null`; phần này sẽ học kỹ ở Buổi 4.
- `ReadLine()` trả `null` khi nguồn nhập kết thúc; chương trình thoát vòng lặp trong trường hợp đó.
- `continue` khi nhập sai đưa chương trình sang lượt menu tiếp theo.
- `break` trong `switch` chỉ thoát khỏi `switch`.
- Chọn 0 làm điều kiện `choice != 0` sai, từ đó kết thúc vòng lặp.
- `GetStatus` chỉ xác định trạng thái; việc nhập và in được xử lý ở nơi gọi.

---

## 10. Lỗi thường gặp

| Lỗi | Nguyên nhân | Cách xử lý |
|---|---|---|
| Viết `if (hp = 0)` | Nhầm gán với so sánh | Dùng `hp == 0` |
| Vòng lặp chạy mãi | Điều kiện không thay đổi hoặc không có cách thoát | Kiểm tra phần cập nhật và điều kiện dừng |
| Truy cập `array[array.Length]` | Nhầm chỉ số cuối | Chỉ số cuối là `Length - 1` |
| Trung bình mất phần lẻ | Chia hai số nguyên | Chuyển một toán hạng sang `double` trước khi chia |
| Gọi `Substring` bị lỗi | Vị trí hoặc độ dài vượt phạm vi | Kiểm tra độ dài chuỗi |
| `ToUpper()` không làm biến đổi giá trị biến | Không sử dụng chuỗi kết quả | Gán lại: `name = name.ToUpper();` |
| Hàm có kiểu trả về nhưng thiếu `return` | Một nhánh có thể đi đến cuối mà chưa trả kết quả | Bổ sung giá trị trả về cho các đường chạy |
| Không gọi được hàm từ `Main` theo mẫu bài học | Hàm chưa khai báo `static` hoặc đặt sai vị trí | Đặt trong `Program`, ngoài `Main`, dùng `static` theo mẫu |

---

## 11. Câu hỏi tự kiểm tra

1. Một chuỗi `if–else if–else` có thể thực hiện nhiều nhánh trong cùng một lần xét không?
2. `while` và `do–while` khác nhau ở đâu?
3. `break` và `continue` khác nhau như thế nào?
4. Mảng có 5 phần tử thì các chỉ số hợp lệ là gì?
5. Khi nào nên dùng `for`, khi nào nên dùng `foreach`?
6. Vì sao tính trung bình bằng `total / count` có thể mất phần thập phân?
7. Sau `name.ToUpper();`, giá trị của biến `name` có tự thay đổi không?
8. Tham số và đối số khác nhau như thế nào?
9. `void` có ý nghĩa gì?
10. Hai phương thức chỉ khác kiểu trả về có được xem là overload không?
11. `int.TryParse` trả về gì khi nhận chuỗi `"abc"`?
12. Trong ví dụ menu, vì sao `break` ở `case 1` không kết thúc vòng lặp?
