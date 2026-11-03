# Buổi 3 — Collection và các kiểu dữ liệu bổ sung trong C#

## 1. Mục tiêu buổi học

Sau buổi học, học viên có thể:

- Phân biệt cách sử dụng `object`, `var` và `dynamic`.
- Dùng `List<T>` để lưu danh sách có thể thêm, xóa phần tử.
- Dùng `Dictionary<TKey, TValue>` để tra cứu dữ liệu theo khóa.
- Hiểu cách hoạt động của Stack, Queue và HashSet.
- Khai báo enum để biểu diễn trạng thái.
- Hiểu sự khác nhau cơ bản giữa struct và class khi gán sang biến khác.

**Kiến thức cần có:** Biến, điều kiện, vòng lặp, mảng và hàm ở Buổi 1–2.

### Thời lượng dự kiến

| Nội dung | Thời gian |
|---|---:|
| Ôn mảng, giới thiệu `var` và `object` | 10 phút |
| `List<T>` | 20 phút |
| `Dictionary<TKey, TValue>` | 20 phút |
| Stack, Queue và HashSet | 15 phút |
| Enum, struct và `#region` | 15 phút |
| Thực hành và giải đáp | 10 phút |
| **Tổng** | **90 phút** |

Nếu chỉ có **60 phút**, tập trung vào List, Dictionary và enum; giới thiệu ngắn các collection còn lại, giao phần so sánh struct/class để tự chạy và quan sát.

Các ví dụ ngắn được đặt trong `Main`, trừ khi ghi rõ vị trí khác. Khi dùng collection, thêm ở đầu file:

```csharp
using System;
using System.Collections.Generic;
```

---

## 2. object, var và dynamic

### 2.1. var — tự suy ra kiểu khi khai báo

Thay vì ghi rõ kiểu:

```csharp
int score = 100;
string playerName = "HIT";
```

Có thể viết:

```csharp
var score = 100;
var playerName = "HIT";
```

Trình biên dịch suy ra:

- `score` có kiểu `int`.
- `playerName` có kiểu `string`.

**`var` không làm biến tự đổi kiểu.**

```csharp
var score = 100;

score = 200;        // Hợp lệ.
// score = "HIT";   // Lỗi: score vẫn là int.
```

Biến dùng `var` cần có giá trị khởi tạo để suy ra kiểu:

```csharp
// var score; // Không hợp lệ.
```

Nên dùng `var` khi kiểu đã rõ từ phần khởi tạo:

```csharp
var damages = new List<int>();
```

### 2.2. object — kiểu cơ sở chung

Biến kiểu `object` có thể giữ dữ liệu thuộc nhiều kiểu khác nhau.

```csharp
object data = 100;
Console.WriteLine(data);

data = "HIT";
Console.WriteLine(data);
```

Tuy nhiên, để dùng dữ liệu như một kiểu cụ thể, thường cần chuyển về kiểu đó:

```csharp
object data = 100;

int score = (int)data;

Console.WriteLine(score + 50); // 150
```

Với ví dụ này:

- Gán `int` vào `object` được gọi là **boxing**.
- Lấy lại giá trị `int` từ object đó được gọi là **unboxing**.

Unboxing phải về đúng kiểu đã được đóng gói:

```csharp
object data = 100;

// double score = (double)data; // Lỗi lúc chạy.
double score = (int)data;       // Lấy int trước, sau đó đổi sang double.
```

Buổi này chỉ cần nhận biết, chưa yêu cầu sử dụng boxing/unboxing trong bài tập.

### 2.3. dynamic — kiểm tra thao tác lúc chạy

```csharp
dynamic data = "HIT";

Console.WriteLine(data.Length); // 3

data = 100;

// Console.WriteLine(data.Length);
// Lỗi lúc chạy vì int không có Length.
```

Với `dynamic`, một số kiểm tra được chuyển từ lúc biên dịch sang lúc chạy.

| Cách khai báo | Điểm cần nhớ |
|---|---|
| `var` | Suy ra kiểu khi biên dịch, kiểu của biến vẫn cố định |
| `object` | Biến có kiểu object; cần chuyển kiểu phù hợp để dùng thành viên riêng |
| `dynamic` | Một số lỗi thao tác chỉ được phát hiện khi chạy |

**Trong các bài tập cơ bản, ưu tiên kiểu cụ thể hoặc `var`; không dùng `dynamic` để tránh xác định kiểu.**

---

## 3. List<T> — danh sách có thể thay đổi số phần tử

### 3.1. Vì sao cần List?

Mảng có độ dài cố định sau khi tạo:

```csharp
int[] scores = new int[3];
```

Nếu cần liên tục thêm hoặc xóa người chơi, dùng List sẽ thuận tiện hơn.

```csharp
List<string> players = new List<string>();
```

`T` biểu thị kiểu phần tử:

- `List<int>`: danh sách số nguyên.
- `List<string>`: danh sách chuỗi.

### 3.2. Ví dụ các thao tác và kết quả

Các ví dụ dưới đây **chạy độc lập**, mỗi đoạn đặt trong `Main` của một project Console, có `using System;` và `using System.Collections.Generic;`. Không ghép nguyên các đoạn vào cùng một Main vì có khai báo trùng tên biến.

`string.Join(", ", items)` dùng để ghép các phần tử thành một dòng dễ quan sát. Hàm này thuộc `string`, không phải hàm của List. Có thể thay bằng foreach để in từng phần tử.

**Nhịp giảng:** Ưu tiên Add, truy cập/cập nhật, Count, Contains, Remove, RemoveAt và duyệt danh sách. Find chỉ giới thiệu; Insert, AddRange, IndexOf, Sort/Reverse và Clear dùng làm ví dụ mở rộng hoặc tự học, không yêu cầu chạy hết trong 18–20 phút.

#### Ví dụ 1 — Add — thêm vào cuối danh sách

```csharp
List<string> items = new List<string>();
items.Add("ammo");
items.Add("shield");
items.Add("ammo");
Console.WriteLine(string.Join(", ", items));
Console.WriteLine(items.Count);
```

**Kết quả:**

```text
ammo, shield, ammo
3
```

Add không trả về giá trị (void). List giữ thứ tự thêm và cho phép phần tử trùng nhau.

#### Ví dụ 2 — Truy cập, cập nhật và Count

```csharp
List<string> items = new List<string> { "ammo", "shield", "ammo" };
Console.WriteLine(items[0]);
items[1] = "potion";
Console.WriteLine(string.Join(", ", items));
Console.WriteLine(items.Count);
```

**Kết quả:**

```text
ammo
ammo, potion, ammo
3
```

Chỉ số bắt đầu từ 0. Gán vào chỉ số đã có thay phần tử, không thêm phần tử mới. Mảng dùng Length; List dùng Count. Count là thuộc tính, không phải hàm; không viết Count(). Chỉ số hợp lệ từ 0 đến Count - 1.

#### Ví dụ 3 — Contains — kiểm tra tồn tại

```csharp
List<string> items = new List<string> { "ammo", "shield" };
Console.WriteLine(items.Contains("ammo"));
Console.WriteLine(items.Contains("laser"));
Console.WriteLine(items.Contains("Ammo"));
```

**Kết quả:**

```text
True
False
False
```

Contains trả bool và không sửa danh sách. So sánh chuỗi mặc định trong ví dụ có phân biệt chữ hoa/thường.

#### Ví dụ 4 — Remove — xóa theo giá trị

```csharp
List<string> items = new List<string> { "ammo", "shield", "ammo" };
bool removed = items.Remove("ammo");
Console.WriteLine(removed);
Console.WriteLine(string.Join(", ", items));
Console.WriteLine(items.Remove("laser"));
Console.WriteLine(items.Count);
```

**Kết quả:**

```text
True
shield, ammo
False
2
```

Remove chỉ xóa lần xuất hiện đầu tiên; trả true nếu xóa được, false nếu không tìm thấy. Xóa mã không có không làm danh sách thay đổi.

#### Ví dụ 5 — RemoveAt — xóa theo chỉ số

```csharp
List<string> items = new List<string> { "ammo", "shield", "potion" };
items.RemoveAt(1);
Console.WriteLine(string.Join(", ", items));
Console.WriteLine(items[1]);
Console.WriteLine(items.Count);
```

**Kết quả:**

```text
ammo, potion
potion
2
```

RemoveAt trả void. Sau khi xóa shield tại chỉ số 1, potion dịch từ chỉ số 2 về 1. Chỉ số không hợp lệ gây lỗi; cần kiểm tra trước nếu chưa chắc danh sách có phần tử ở vị trí đó.

#### Ví dụ 6 — Duyệt bằng for và foreach

```csharp
List<string> items = new List<string> { "ammo", "shield" };
for (int i = 0; i < items.Count; i++)
{
    Console.WriteLine($"{i}: {items[i]}");
}
foreach (string item in items)
{
    Console.WriteLine(item);
}
```

**Kết quả:**

```text
0: ammo
1: shield
ammo
shield
```

for cho phép dùng chỉ số; foreach đọc lần lượt các phần tử. Không thêm/xóa phần tử của List khi foreach đang duyệt chính List đó.

#### Ví dụ 7 — Find — tìm phần tử đầu tiên thỏa điều kiện

```csharp
List<int> scores = new List<int> { 40, 120, 150, 0 };
int found = scores.Find(score => score > 100);
int missing = scores.Find(score => score > 999);
Console.WriteLine(found);
Console.WriteLine(missing);
```

**Kết quả:**

```text
120
0
```

Đọc score => score > 100 là nhận một điểm số và kiểm tra nó lớn hơn 100. Find lấy phần tử đầu tiên phù hợp, không lấy mọi phần tử. Khi không có, trả giá trị mặc định của kiểu phần tử; int là 0. Vì 0 cũng có thể là dữ liệu hợp lệ, chỉ nhìn kết quả 0 chưa đủ biết có tìm thấy hay không. Lambda sẽ học kỹ ở buổi 5.

#### Ví dụ 8 — Insert — chèn tại vị trí chỉ định

```csharp
List<string> items = new List<string> { "ammo", "potion" };
items.Insert(1, "shield");
Console.WriteLine(string.Join(", ", items));
Console.WriteLine(items.Count);
```

**Kết quả:**

```text
ammo, shield, potion
3
```

Insert trả void, chèn phần tử và dịch các phần tử phía sau sang phải. Chỉ số chèn hợp lệ từ 0 đến Count, gồm cả Count để chèn cuối.

#### Ví dụ 9 — AddRange — thêm nhiều phần tử vào cuối

```csharp
List<string> items = new List<string> { "ammo" };
items.AddRange(new string[] { "shield", "potion" });
Console.WriteLine(string.Join(", ", items));
Console.WriteLine(items.Count);
```

**Kết quả:**

```text
ammo, shield, potion
3
```

AddRange trả void. Có thể truyền một mảng hoặc collection phù hợp; ví dụ thêm hai phần tử theo thứ tự của mảng.

#### Ví dụ 10 — IndexOf — tìm chỉ số theo giá trị

```csharp
List<string> items = new List<string> { "ammo", "shield", "ammo" };
Console.WriteLine(items.IndexOf("ammo"));
Console.WriteLine(items.IndexOf("shield"));
Console.WriteLine(items.IndexOf("laser"));
```

**Kết quả:**

```text
0
1
-1
```

IndexOf trả chỉ số của lần xuất hiện đầu tiên hoặc -1 khi không thấy. Không dùng kết quả -1 làm chỉ số để đọc hay xóa phần tử.

#### Ví dụ 11 — Sort và Reverse — sắp xếp và đảo thứ tự

```csharp
List<int> scores = new List<int> { 30, 10, 20 };
scores.Sort();
Console.WriteLine(string.Join(", ", scores));
scores.Reverse();
Console.WriteLine(string.Join(", ", scores));
```

**Kết quả:**

```text
10, 20, 30
30, 20, 10
```

Sort và Reverse đều trả void và sửa trực tiếp List. Sort() với int sắp tăng dần. Reverse() chỉ đảo thứ tự hiện tại; nó không tự sắp xếp giảm dần nếu trước đó danh sách chưa được sắp tăng dần.

#### Ví dụ 12 — Clear — xóa toàn bộ phần tử

```csharp
List<string> items = new List<string> { "ammo", "shield" };
items.Clear();
Console.WriteLine(items.Count);
Console.WriteLine(items.Contains("ammo"));
items.Add("potion");
Console.WriteLine(items[0]);
```

**Kết quả:**

```text
0
False
potion
```

Clear trả void và làm Count bằng 0. List vẫn tồn tại, có thể thêm phần tử trở lại; không đọc items[0] ngay sau Clear khi chưa thêm phần tử.

#### Bảng nhắc nhanh về giá trị trả về

| Thành viên | Giá trị trả về | Có thay đổi List không? |
| --- | --- | --- |
| Add, AddRange, Insert | void | Có |
| Remove | bool: có xóa được hay không | Có nếu tìm thấy |
| RemoveAt, Clear | void | Có |
| Contains | bool: có tồn tại hay không | Không |
| IndexOf | int: chỉ số đầu tiên hoặc -1 | Không |
| Find | Phần tử đầu tiên phù hợp hoặc giá trị mặc định | Không |
| Sort, Reverse | void | Có |
| Count (thuộc tính) | int: số phần tử | Không |

Các hàm không tự in ra màn hình. Các dòng kết quả trong ví dụ xuất hiện do gọi Console.WriteLine; với hàm trả void, hãy gọi hàm trước rồi in trạng thái List sau đó.

**Thực hành nhanh:** Tạo List gồm An, Bình, Chi; thêm Dũng, xóa Bình rồi in Count và các tên còn lại. Kết quả: 3 người, theo thứ tự An, Chi, Dũng.

---

## 4. Dictionary<TKey, TValue> — lưu dữ liệu theo khóa

### 4.1. Khái niệm

Dictionary lưu các cặp **khóa – giá trị**.

Ví dụ:

| Khóa: mã vật phẩm | Giá trị: số lượng |
|---|---:|
| `ammo` | 10 |
| `shield` | 2 |
| `potion` | 5 |

Khai báo:

```csharp
Dictionary<string, int> inventory =
    new Dictionary<string, int>();
```

Trong đó:

- `string` là kiểu của khóa.
- `int` là kiểu của giá trị.
- Mỗi khóa là duy nhất.

### 4.2. Thêm và cập nhật

```csharp
inventory.Add("ammo", 10);
inventory.Add("shield", 2);

inventory["potion"] = 5;
inventory["ammo"] = 20;
```

Phân biệt:

- `Add(key, value)`: thêm mới; trùng khóa sẽ gây lỗi.
- `dictionary[key] = value`: thêm nếu chưa có, cập nhật nếu đã có.

### 4.3. Đọc giá trị

Nếu chắc chắn khóa tồn tại:

```csharp
Console.WriteLine(inventory["ammo"]); // 20
```

Đọc bằng khóa không tồn tại sẽ gây lỗi.

### 4.4. ContainsKey — kiểm tra khóa

```csharp
if (inventory.ContainsKey("shield"))
{
    Console.WriteLine(inventory["shield"]);
}
else
{
    Console.WriteLine("Khong co shield.");
}
```

### 4.5. TryGetValue — tra cứu có kiểm tra

```csharp
if (inventory.TryGetValue("ammo", out int quantity))
{
    Console.WriteLine($"So luong dan: {quantity}");
}
else
{
    Console.WriteLine("Khong co ammo.");
}
```

- Trả về `true` khi tìm thấy khóa.
- `quantity` nhận giá trị tương ứng.
- Nếu không tìm thấy, trả về `false`; với ví dụ này, `quantity` nhận 0.

`out` là cách nhận kết quả từ phương thức; sẽ học kỹ hơn ở Buổi 4.

### 4.6. Xóa và duyệt

```csharp
inventory.Remove("shield");

foreach (var item in inventory)
{
    Console.WriteLine($"{item.Key}: {item.Value}");
}
```

Trong vòng lặp:

- `item.Key`: khóa.
- `item.Value`: giá trị.

Không dựa vào thứ tự duyệt Dictionary để biểu diễn thứ hạng hoặc thứ tự người đến.

### 4.7. Ví dụ đếm số lượng vật phẩm

```csharp
string[] collectedItems = { "ammo", "shield", "ammo" };

Dictionary<string, int> counts =
    new Dictionary<string, int>();

foreach (string item in collectedItems)
{
    if (counts.ContainsKey(item))
    {
        counts[item]++;
    }
    else
    {
        counts.Add(item, 1);
    }
}

Console.WriteLine(counts["ammo"]);   // 2
Console.WriteLine(counts["shield"]); // 1
```

**Thực hành nhanh:** Thêm `"potion"` và một lượt `"shield"` vào dữ liệu trên, sau đó kiểm tra số lượng từng loại.

---

## 5. Stack<T> và Queue<T>

### 5.1. Stack — vào sau, ra trước

Có thể hình dung Stack như chồng đĩa: đĩa đặt lên sau được lấy ra trước.

Các thao tác chính:

- `Push`: thêm vào đỉnh.
- `Pop`: lấy và xóa phần tử ở đỉnh.
- `Peek`: xem phần tử ở đỉnh, không xóa.
- `Count`: số phần tử.

```csharp
Stack<string> history = new Stack<string>();

history.Push("Di chuyen");
history.Push("Ban");

Console.WriteLine(history.Peek()); // Ban
Console.WriteLine(history.Pop());  // Ban
Console.WriteLine(history.Pop());  // Di chuyen
```

**Ứng dụng:** Lưu lịch sử thao tác để thực hiện undo.

### 5.2. Queue — vào trước, ra trước

Queue giống hàng người chờ mua vé: ai đến trước được phục vụ trước.

Các thao tác chính:

- `Enqueue`: thêm vào cuối hàng.
- `Dequeue`: lấy và xóa phần tử đầu hàng.
- `Peek`: xem phần tử đầu hàng, không xóa.
- `Count`: số phần tử.

```csharp
Queue<string> waitingPlayers = new Queue<string>();

waitingPlayers.Enqueue("An");
waitingPlayers.Enqueue("Binh");

Console.WriteLine(waitingPlayers.Dequeue()); // An
Console.WriteLine(waitingPlayers.Dequeue()); // Binh
```

**Ứng dụng:** Hàng chờ ghép trận, các yêu cầu cần xử lý theo thứ tự.

### 5.3. Kiểm tra trước khi lấy

`Pop`, `Dequeue` và `Peek` gây lỗi khi collection rỗng.

```csharp
if (waitingPlayers.Count > 0)
{
    Console.WriteLine(waitingPlayers.Dequeue());
}
else
{
    Console.WriteLine("Hang cho rong.");
}
```

| Cấu trúc | Quy tắc | Ví dụ |
|---|---|---|
| Stack | Vào sau, ra trước — LIFO | Undo thao tác gần nhất |
| Queue | Vào trước, ra trước — FIFO | Phục vụ người đến trước |

---

## 6. HashSet<T> — tập hợp không trùng

HashSet phù hợp khi cần lưu các giá trị duy nhất hoặc kiểm tra một giá trị đã tồn tại chưa.

```csharp
HashSet<string> achievements = new HashSet<string>();

bool firstAdd = achievements.Add("first_win");
bool secondAdd = achievements.Add("first_win");

Console.WriteLine(firstAdd);           // True
Console.WriteLine(secondAdd);          // False
Console.WriteLine(achievements.Count); // 1
```

Thêm một giá trị đã có không tạo thêm phần tử.

### Kiểm tra và xóa

```csharp
if (achievements.Contains("first_win"))
{
    Console.WriteLine("Da mo khoa thanh tuu.");
}

achievements.Remove("first_win");
```

**Ghi nhớ:**

- Không truy cập HashSet bằng chỉ số như `achievements[0]`.
- Không dựa vào thứ tự duyệt của HashSet.
- Với chuỗi, mặc định phân biệt hoa/thường.
- Phù hợp cho danh sách mã thành tựu, mã vật phẩm đã khám phá hoặc kiểm tra trùng.

---

## 7. Enum — đặt tên cho trạng thái

Thay vì dùng những số khó nhớ:

```csharp
int gameState = 1;
```

Có thể đặt tên rõ ràng cho các trạng thái bằng enum.

Đặt khai báo sau **ngoài `Main`**, chẳng hạn phía trên class `Program`:

```csharp
enum GameState
{
    Menu,
    Playing,
    Paused,
    GameOver
}
```

Sử dụng bên trong `Main`:

```csharp
GameState state = GameState.Playing;

if (state == GameState.Playing)
{
    Console.WriteLine("Dang choi.");
}
```

Có thể kết hợp với `switch`:

```csharp
switch (state)
{
    case GameState.Menu:
        Console.WriteLine("Man hinh chinh.");
        break;

    case GameState.Playing:
        Console.WriteLine("Dang choi.");
        break;

    case GameState.Paused:
        Console.WriteLine("Tam dung.");
        break;

    case GameState.GameOver:
        Console.WriteLine("Ket thuc.");
        break;
}
```

Enum giúp tên trạng thái dễ đọc và hạn chế việc dùng nhầm các mã số.

---

## 8. Struct và class — giới thiệu hành vi sao chép

Buổi này chỉ cần quan sát sự khác nhau khi gán sang biến khác. Cách thiết kế class sẽ học ở Buổi 4.

### 8.1. Struct: sao chép giá trị

Đặt khai báo ngoài `Main`:

```csharp
struct GridPosition
{
    public int X;
    public int Y;
}
```

Sử dụng trong `Main`:

```csharp
GridPosition first = new GridPosition();
first.X = 2;
first.Y = 3;

GridPosition second = first;
second.X = 99;

Console.WriteLine(first.X);  // 2
Console.WriteLine(second.X); // 99
```

`second = first` sao chép giá trị của struct. Đổi `second.X` không làm `first.X` thay đổi.

### 8.2. Class: sao chép tham chiếu

Đặt khai báo ngoài `Main`:

```csharp
class PlayerData
{
    public int Hp;
}
```

Sử dụng trong `Main`:

```csharp
PlayerData first = new PlayerData();
first.Hp = 100;

PlayerData second = first;
second.Hp = 50;

Console.WriteLine(first.Hp);  // 50
Console.WriteLine(second.Hp); // 50
```

Hai biến cùng tham chiếu đến một đối tượng. Sửa HP qua `second` thì đọc qua `first` cũng thấy giá trị mới.

| Struct | Class |
|---|---|
| Là kiểu giá trị | Là kiểu tham chiếu |
| Phép gán sao chép giá trị | Phép gán sao chép tham chiếu |
| Thường dùng cho dữ liệu mang ý nghĩa một giá trị, như tọa độ | Thường dùng cho đối tượng có danh tính và trạng thái, như người chơi |

Không nên ghi nhớ bằng quy tắc “struct luôn nằm trên stack, class luôn nằm trên heap”. Điều cần nắm ở đây là **hành vi sao chép**.

Các ví dụ dùng field `public` để dễ quan sát. Buổi 4 sẽ học cách bảo vệ dữ liệu bằng field private và property.

---

## 9. #region — thu gọn code trong IDE

```csharp
#region Inventory

// Các phương thức quản lý kho vật phẩm.

#endregion
```

`#region` giúp nhóm và thu gọn một phần code trong IDE.

Nó không:

- Tạo phạm vi biến mới.
- Thay đổi cách chương trình chạy.
- Thay thế việc chia hàm hoặc chia class.

Chỉ dùng khi giúp tìm nội dung trong file dễ hơn.

---

## 10. Chọn cấu trúc dữ liệu phù hợp

| Nhu cầu | Cấu trúc phù hợp |
|---|---|
| Số phần tử cố định, truy cập theo chỉ số | Mảng |
| Danh sách thường xuyên thêm hoặc xóa | `List<T>` |
| Tra cứu dữ liệu theo mã hoặc tên khóa | `Dictionary<TKey, TValue>` |
| Lấy thao tác mới nhất trước | `Stack<T>` |
| Xử lý theo thứ tự đến | `Queue<T>` |
| Lưu giá trị duy nhất, kiểm tra trùng | `HashSet<T>` |
| Biểu diễn một nhóm trạng thái có tên | Enum |

---

## 11. Thực hành cuối buổi — Đếm vật phẩm

Cho dữ liệu:

```csharp
string[] items =
{
    "ammo",
    "shield",
    "ammo",
    "potion",
    "shield",
    "ammo"
};
```

### Yêu cầu

1. Dùng Dictionary đếm số lượng từng loại.
2. In số lượng của `ammo`, `shield`, `potion` theo thứ tự này.
3. Dùng `TryGetValue` để tra mã `laser`.
4. Nếu không có `laser`, in `Khong co laser`.

### Kết quả mong đợi

```text
ammo: 3
shield: 2
potion: 1
Khong co laser
```

Nếu còn thời gian, dùng HashSet đếm số loại vật phẩm khác nhau. Kết quả là **3 loại**.

---

## 12. Câu hỏi tự kiểm tra

1. `var` có cho phép một biến đang là số nguyên đổi thành chuỗi không?
2. Mảng dùng `Length`, List dùng thuộc tính gì?
3. `Remove` và `RemoveAt` khác nhau như thế nào?
4. Khi nào nên dùng Dictionary thay cho List?
5. `Add` một khóa đã tồn tại vào Dictionary có hợp lệ không?
6. Stack và Queue khác nhau ở thứ tự lấy phần tử như thế nào?
7. Trước khi gọi `Pop` hoặc `Dequeue`, cần kiểm tra điều gì?
8. Thêm hai lần cùng một giá trị vào HashSet thì có bao nhiêu phần tử?
9. Enum giúp đoạn code về trạng thái dễ đọc hơn như thế nào?
10. Vì sao hai ví dụ sao chép struct và class cho kết quả khác nhau?
