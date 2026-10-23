# Buổi 1 — Công cụ, Git và nền tảng C#

[Bài tập về nhà →](BTVN.md)

## 1. Mục tiêu

Sau buổi học, bạn có thể tạo và chạy Console project; nhập, xử lý, xuất dữ liệu đơn giản; giải thích kiểu dữ liệu, biến, hằng và phép chuyển đổi kiểu; lưu bài bằng Git và đồng bộ với GitHub.

## 2. Giới thiệu lớp và công cụ

5 buổi đầu học C# qua Console, sau đó chuyển sang Unity. C# là ngôn ngữ lập trình; .NET SDK cung cấp công cụ tạo, biên dịch và chạy project. Unity dùng C# cho script nhưng có môi trường và API riêng.

| Công cụ | Vai trò |
| --- | --- |
| .NET SDK | Biên dịch và chạy C# Console |
| Visual Studio hoặc JetBrains Rider | IDE: viết code, gợi ý cú pháp, chạy và debug |
| Git | Quản lý lịch sử thay đổi trên máy |
| GitHub | Lưu repo trực tuyến và cộng tác |
| Git Bash | Terminal tiện dùng Git trên Windows |
| GitHub Desktop hoặc Fork | Giao diện trực quan cho thao tác Git |

Cài .NET SDK từ [Microsoft](https://dotnet.microsoft.com/download), chọn IDE từ [Visual Studio](https://visualstudio.microsoft.com/) hoặc [Rider](https://www.jetbrains.com/rider/). Với Visual Studio, chọn workload phát triển .NET cho desktop. Cài [Git for Windows](https://git-scm.com/downloads/win); có thể cài thêm [GitHub Desktop](https://desktop.github.com/) hoặc [Fork](https://git-fork.com/). Không bắt buộc dùng đồng thời cả hai ứng dụng giao diện Git.

Trong IDE, làm quen cửa sổ quản lý project, file `Program.cs`, nút Run, danh sách lỗi và terminal. Khi gặp lỗi biên dịch, đọc tên file và số dòng trong thông báo trước khi sửa.

## 3. Git: từ máy cá nhân đến GitHub

Git khác GitHub. Một commit là mốc lưu thay đổi trong repo trên máy; `push` mới gửi commit lên repo từ xa.

Luồng cơ bản: sửa file → `add` để chọn thay đổi → `commit` để lưu mốc → `push` để đồng bộ.

Ví dụ dưới đây thao tác trên repo cá nhân đã tạo trên GitHub. Thay URL bằng URL thật của bạn:

```bash
git clone https://github.com/TEN_TAI_KHOAN/TEN_REPO.git
cd TEN_REPO
git status
git add README.md
git commit -m "docs: them gioi thieu ban than"
git push
git pull
```

- `clone`: tải repo và cấu hình liên kết `origin`.
- `status`: xem file mới, file sửa và các thay đổi đã chọn.
- `add`: chọn nội dung cho commit tiếp theo; file phải tồn tại và có thay đổi.
- `commit`: lưu các thay đổi đã chọn vào lịch sử trên máy.
- `push`: gửi commit; cần quyền ghi và đăng nhập GitHub theo hướng dẫn công cụ.
- `pull`: lấy thay đổi từ xa và tích hợp vào nhánh hiện tại. Nên commit công việc trước khi pull.

Nếu Git chưa biết người tạo commit, cấu hình tên và email thật của bạn:

```bash
git config --global user.name "Ten cua ban"
git config --global user.email "email-cua-ban@example.com"
```

Không lưu mật khẩu hoặc token trong mã nguồn. Thêm `bin/`, `obj/` vào `.gitignore` trước khi chọn file để commit.

Trong GitHub Desktop/Fork, các thao tác tương ứng là Clone, xem Changes, chọn file, nhập nội dung commit, Commit, Push và Pull. Hãy xem danh sách file trước mỗi commit.

### Conflict đơn giản

Conflict có thể xảy ra khi hai nhánh sửa cùng một vùng và Git không tự gộp được. File có thể chứa:

```text
<<<<<<< HEAD
Ten lop: HIT C#
=======
Ten lop: HIT C# va Unity
>>>>>>> ten-nhanh
```

Đọc cả hai phần, sửa thành nội dung cuối cùng mong muốn rồi xóa các dấu `<<<<<<<`, `=======`, `>>>>>>>`. Sau đó:

```bash
git add README.md
git commit -m "docs: gop noi dung gioi thieu"
git push
```

Đây là ví dụ hoàn tất một merge thông thường; nếu công cụ báo đang rebase, dùng thao tác Continue Rebase theo trạng thái công cụ. Không chọn giữ một phía một cách máy móc.

## 4. Chương trình C# đầu tiên

```csharp
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("Hello HIT!");
    }
}
```

- `using System;`: cho phép viết ngắn tên kiểu trong namespace `System`, chẳng hạn `Console`.
- `class Program`: định nghĩa lớp chứa phương thức `Main`.
- `static void Main()`: điểm bắt đầu của chương trình theo cách viết này.
- `{ }`: nhóm các thành phần hoặc câu lệnh thành một khối.
- `;`: kết thúc nhiều loại câu lệnh, chẳng hạn gán hoặc gọi hàm. Không phải mọi dòng đều có `;`.
- `//`: chú thích một dòng; `/* ... */`: chú thích nhiều dòng; `///`: chú thích tài liệu XML.

## 5. Biến, hằng và kiểu dữ liệu

Biến có tên và kiểu; giá trị của nó có thể thay đổi. Biến cục bộ phải được gán trước khi đọc.

```csharp
int hp = 100;
hp = 80;
const int MaxHp = 100;
```

`const` phải có giá trị xác định ở thời điểm biên dịch và không được gán lại. `#define` là ký hiệu phục vụ biên dịch có điều kiện, không phải hằng số dữ liệu.

| Kiểu | Dùng cho | Ví dụ |
| --- | --- | --- |
| `int` | Số nguyên trong khoảng −2.147.483.648 đến 2.147.483.647 | `int score = 120;` |
| `float` | Số thực độ chính xác đơn; literal thường cần `f` | `float speed = 3.5f;` |
| `double` | Số thực độ chính xác cao hơn `float` | `double distance = 12.5;` |
| `bool` | Đúng/sai | `bool isAlive = true;` |
| `char` | Một đơn vị mã UTF-16, dùng nháy đơn | `char rank = 'A';` |
| `string` | Văn bản, dùng nháy kép | `string name = "HIT";` |

Tên biến phân biệt hoa/thường. Không bắt đầu bằng số; không có khoảng trắng; tránh từ khóa. C# cho phép nhiều ký tự Unicode trong tên nhưng lớp thống nhất dùng tên không dấu, có nghĩa, như `playerName`, `currentHp`.

## 6. Nhập và xuất dữ liệu

`Console.Write` in mà không tự xuống dòng; `Console.WriteLine` in rồi xuống dòng. `Console.ReadLine` đọc một dòng và trả về chuỗi, hoặc `null` khi hết nguồn nhập.

```csharp
using System;

class Program
{
    static void Main()
    {
        Console.Write("Nhap ten: ");
        string name = Console.ReadLine() ?? "";
        Console.Write("Nhap tuoi: ");
        int age = int.Parse(Console.ReadLine() ?? "0");
        Console.WriteLine("Xin chao " + name);
        Console.WriteLine("Tuoi: " + age);
    }
}
```

`?? ""` dùng chuỗi rỗng nếu kết quả bên trái là `null`; sẽ học kỹ ở buổi 4. `Parse` chuyển chuỗi số sang số; nhập `abc` sẽ gây lỗi. Trong ví dụ nhập môn, giả sử người dùng nhập đúng định dạng; buổi 2 dùng `TryParse` và điều kiện để kiểm tra.

`Console.Read()` trả về mã của ký tự tiếp theo dưới dạng `int` hoặc `-1` khi hết dữ liệu; không chỉ giới hạn ASCII. `Console.ReadKey()` trả về thông tin phím, không phải một chuỗi nhập hoàn chỉnh.

## 7. Toán tử

| Nhóm | Toán tử | Ví dụ |
| --- | --- | --- |
| Số học | `+ - * / %` | `17 % 5` bằng `2` |
| Gán | `= += -= *= /= %=` | `hp -= 10;` |
| Tăng/giảm | `++ --` | `score++;` |
| So sánh | `== != > < >= <=` | `hp > 0` trả về `bool` |
| Logic | `&& || !` | `hp > 0 && hasAmmo` |

`=` là gán, `==` là so sánh. `&&` cần cả hai điều kiện đúng; `||` cần ít nhất một điều kiện đúng; `!` đảo đúng/sai. `&&`/`||` có thể bỏ qua vế phải khi đã xác định được kết quả.

```csharp
int a = 7, b = 2;
Console.WriteLine(a / b);         // 3: chia hai số nguyên
Console.WriteLine((double)a / b); // 3.5: chia số thực
Console.WriteLine(a % b);         // 1
```

Phép nhân/chia/chia dư ưu tiên hơn cộng/trừ. Dùng ngoặc để thể hiện ý định: `(a + b) / 2.0`. Không chia số nguyên cho 0. Với số âm, phép chia nguyên làm tròn về 0.

## 8. Chuyển đổi kiểu

```csharp
int score = 100;
double expanded = score;          // Ngầm định: int -> double
double value = 9.8;
int truncated = (int)value;       // Tường minh: 9, cắt phần thập phân
int rounded = Convert.ToInt32(value); // 10, làm tròn
int parsed = int.Parse("123");   // Chuyển chuỗi thành số
```

Ép `(int)` và `Convert.ToInt32(double)` khác nhau: một bên cắt phần lẻ, một bên làm tròn đến số nguyên gần nhất; khi đúng giữa hai số thì `Convert` làm tròn về số chẵn gần nhất. Chuyển đổi tường minh có thể mất dữ liệu; không thử chuyển giá trị vượt phạm vi kiểu đích trong bài nhập môn. Không dùng `(int)"123"` để chuyển chuỗi thành số.

Để nhập số thực bằng dấu chấm bất kể thiết lập vùng của máy, dùng:

```csharp
double speed = double.Parse(
    Console.ReadLine() ?? "0",
    System.Globalization.CultureInfo.InvariantCulture);
```

Nếu chỉ dùng `double.Parse(text)`, định dạng được hiểu theo culture hiện hành của máy. Chuỗi rỗng vẫn không phải số hợp lệ.

## 9. Tự kiểm tra

- Giải thích vì sao `5 / 2` khác `5 / 2.0`.
- Phân biệt `char` và `string`, `=` và `==`, commit và push.
- Sửa ví dụ nhập tên để in thêm HP, tốc độ và trạng thái còn sống.
- Đọc một lỗi biên dịch do thiếu `;` và tìm đúng dòng cần sửa.

## Tài liệu đọc thêm

- [Microsoft — Built-in types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/built-in-types)
- [Microsoft — Casting and type conversions](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/types/casting-and-type-conversions)
- [Git — Recording changes](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)
