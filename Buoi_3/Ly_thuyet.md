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

### 4.2. Ví dụ các thao tác và kết quả

Mỗi ví dụ bên dưới **chạy độc lập** trong Main của project Console, có `using System;` và `using System.Collections.Generic;`. Không ghép các ví dụ nguyên trạng vào cùng Main vì có thể trùng tên biến. Khi có khai báo enum/struct/class, đặt khai báo đó **ngoài Main**, cùng cấp với class Program.

Các hàm không tự in ra màn hình; kết quả dưới mỗi ví dụ do Console.WriteLine in giá trị trả về hoặc trạng thái sau thao tác. Count là thuộc tính, không viết Count().

#### Ví dụ 1 — Add, bộ chỉ mục và Count

```csharp
Dictionary<string, int> inventory = new Dictionary<string, int>();
inventory.Add("ammo", 10);
inventory["shield"] = 2;
inventory["ammo"] = 20;
Console.WriteLine(inventory["ammo"]);
Console.WriteLine(inventory["shield"]);
Console.WriteLine(inventory.Count);
```

**Kết quả:**

```text
20
2
2
```

Add trả void, chỉ thêm khóa mới; trùng khóa gây ArgumentException. Gán qua [key] thêm nếu chưa có, cập nhật nếu đã có. Đọc qua [key] cần khóa tồn tại, nếu không sẽ có KeyNotFoundException. Count là số cặp khóa–giá trị, không phải tổng số vật phẩm.

#### Ví dụ 2 — ContainsKey — kiểm tra khóa

```csharp
Dictionary<string, int> inventory = new Dictionary<string, int>
{
    { "ammo", 10 }, { "shield", 2 }
};
Console.WriteLine(inventory.ContainsKey("ammo"));
Console.WriteLine(inventory.ContainsKey("laser"));
Console.WriteLine(inventory.ContainsKey("Ammo"));
```

**Kết quả:**

```text
True
False
False
```

ContainsKey trả bool, không sửa dữ liệu. Các khóa string trong ví dụ phân biệt chữ hoa/thường.

#### Ví dụ 3 — TryGetValue — tìm khóa và lấy giá trị

```csharp
Dictionary<string, int> inventory = new Dictionary<string, int>
{
    { "ammo", 10 }, { "shield", 0 }
};
bool foundAmmo = inventory.TryGetValue("ammo", out int ammo);
bool foundLaser = inventory.TryGetValue("laser", out int laser);
bool foundShield = inventory.TryGetValue("shield", out int shield);
Console.WriteLine($"ammo: {foundAmmo}, {ammo}");
Console.WriteLine($"laser: {foundLaser}, {laser}");
Console.WriteLine($"shield: {foundShield}, {shield}");
```

**Kết quả:**

```text
ammo: True, 10
laser: False, 0
shield: True, 0
```

TryGetValue trả bool và đưa giá trị vào biến out. Nếu không tìm thấy, out nhận giá trị mặc định của TValue, với int là 0. Shield có thật và số lượng bằng 0; laser không có nhưng cũng nhận 0. Vì vậy cần đọc bool để phân biệt. out sẽ học kỹ ở buổi 4.

#### Ví dụ 4 — Remove — xóa theo khóa

```csharp
Dictionary<string, int> inventory = new Dictionary<string, int>
{
    { "ammo", 10 }, { "shield", 2 }
};
Console.WriteLine(inventory.Remove("shield"));
Console.WriteLine(inventory.Remove("laser"));
Console.WriteLine(inventory.ContainsKey("shield"));
Console.WriteLine(inventory.Count);
```

**Kết quả:**

```text
True
False
False
1
```

Remove(key) trả true nếu tìm thấy và xóa cặp tương ứng, false nếu không có khóa. Xóa khóa đồng thời xóa giá trị đi kèm.

#### Ví dụ 5 — foreach — đọc Key và Value

```csharp
Dictionary<string, int> inventory = new Dictionary<string, int>
{
    { "ammo", 10 }, { "shield", 2 }
};
int total = 0;
foreach (var entry in inventory)
{
    total += entry.Value;
}
Console.WriteLine($"Tong so luong: {total}");
string[] displayOrder = { "ammo", "shield" };
foreach (string key in displayOrder)
{
    Console.WriteLine($"{key}: {inventory[key]}");
}
```

**Kết quả:**

```text
Tong so luong: 12
ammo: 10
shield: 2
```

Khi duyệt Dictionary, entry.Key là khóa, entry.Value là giá trị. Ví dụ cộng các Value nên không phụ thuộc thứ tự duyệt. Muốn thứ tự in xác định, dùng mảng khóa như displayOrder; không dựa vào thứ tự duyệt Dictionary để biểu diễn thứ hạng.

#### Ví dụ 6 — Clear — xóa toàn bộ cặp dữ liệu

```csharp
Dictionary<string, int> inventory = new Dictionary<string, int>
{
    { "ammo", 10 }, { "shield", 2 }
};
inventory.Clear();
Console.WriteLine(inventory.Count);
Console.WriteLine(inventory.ContainsKey("ammo"));
inventory.Add("potion", 3);
Console.WriteLine(inventory["potion"]);
```

**Kết quả:**

```text
0
False
3
```

Clear trả void. Dictionary trở thành rỗng nhưng vẫn có thể thêm dữ liệu mới.

#### Ví dụ 7 — Đếm số lần xuất hiện bằng Dictionary

```csharp
string[] drops = { "ammo", "shield", "ammo" };
Dictionary<string, int> counts = new Dictionary<string, int>();
foreach (string item in drops)
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
Console.WriteLine($"ammo: {counts["ammo"]}");
Console.WriteLine($"shield: {counts["shield"]}");
Console.WriteLine($"So loai: {counts.Count}");
```

**Kết quả:**

```text
ammo: 2
shield: 1
So loai: 2
```

Đã gặp khóa thì tăng số lượng; chưa có thì thêm với số lượng 1. Ba lượt nhặt tạo hai khóa, tương ứng hai loại. Đây là demo của người dạy; phần thực hành giữ nguyên.

**Thực hành nhanh:** Thêm `"potion"` và một lượt `"shield"` vào dữ liệu trên, sau đó kiểm tra số lượng từng loại.

---

## 5. Stack<T> và Queue<T>

### 5.1. Stack — vào sau, ra trước (LIFO)

Stack giống chồng đĩa: đặt sau lấy trước.

Mỗi ví dụ bên dưới **chạy độc lập** trong Main của project Console, có `using System;` và `using System.Collections.Generic;`. Không ghép các ví dụ nguyên trạng vào cùng Main vì có thể trùng tên biến. Khi có khai báo enum/struct/class, đặt khai báo đó **ngoài Main**, cùng cấp với class Program.

Các hàm không tự in ra màn hình; kết quả dưới mỗi ví dụ do Console.WriteLine in giá trị trả về hoặc trạng thái sau thao tác. Count là thuộc tính, không viết Count().

#### Ví dụ 1 — Push và Count — thêm vào đỉnh

```csharp
Stack<string> history = new Stack<string>();
history.Push("Di chuyen");
history.Push("Ban");
Console.WriteLine(history.Count);
Console.WriteLine(history.Peek());
```

**Kết quả:**

```text
2
Ban
```

Push trả void, thêm vào đỉnh. Phần tử thêm sau cùng là Ban nên nằm ở đỉnh. Count là thuộc tính số phần tử.

#### Ví dụ 2 — Peek và Pop — xem khác với lấy ra

```csharp
Stack<string> history = new Stack<string>();
history.Push("Di chuyen");
history.Push("Ban");
Console.WriteLine(history.Peek());
Console.WriteLine(history.Count);
Console.WriteLine(history.Pop());
Console.WriteLine(history.Count);
Console.WriteLine(history.Pop());
Console.WriteLine(history.Count);
```

**Kết quả:**

```text
Ban
2
Ban
1
Di chuyen
0
```

Peek trả phần tử ở đỉnh nhưng không xóa. Pop trả phần tử ở đỉnh và xóa nó. Thứ tự lấy là vào sau, ra trước (LIFO). Stack có thể lưu lịch sử, còn hành vi undo phải lập trình thêm.

#### Ví dụ 3 — Contains, Clear và kiểm tra rỗng

```csharp
Stack<string> history = new Stack<string>();
history.Push("Ban");
Console.WriteLine(history.Contains("Ban"));
Console.WriteLine(history.Contains("Nhay"));
history.Clear();
Console.WriteLine(history.Count);
if (history.Count > 0)
{
    Console.WriteLine(history.Pop());
}
else
{
    Console.WriteLine("Lich su rong");
}
```

**Kết quả:**

```text
True
False
0
Lich su rong
```

Contains trả bool, Clear trả void. Pop/Peek khi rỗng gây InvalidOperationException; kiểm tra Count > 0 trước khi gọi nếu chưa biết có dữ liệu.

### 5.2. Queue — vào trước, ra trước (FIFO)

Queue giống hàng người chờ: đến trước được phục vụ trước.

Mỗi ví dụ bên dưới **chạy độc lập** trong Main của project Console, có `using System;` và `using System.Collections.Generic;`. Không ghép các ví dụ nguyên trạng vào cùng Main vì có thể trùng tên biến. Khi có khai báo enum/struct/class, đặt khai báo đó **ngoài Main**, cùng cấp với class Program.

Các hàm không tự in ra màn hình; kết quả dưới mỗi ví dụ do Console.WriteLine in giá trị trả về hoặc trạng thái sau thao tác. Count là thuộc tính, không viết Count().

#### Ví dụ 1 — Enqueue và Count — thêm vào cuối hàng

```csharp
Queue<string> waiting = new Queue<string>();
waiting.Enqueue("An");
waiting.Enqueue("Binh");
Console.WriteLine(waiting.Count);
Console.WriteLine(waiting.Peek());
```

**Kết quả:**

```text
2
An
```

Enqueue trả void. An đến trước nên đứng đầu hàng; Count cho biết có hai phần tử.

#### Ví dụ 2 — Peek và Dequeue — xem khác với phục vụ

```csharp
Queue<string> waiting = new Queue<string>();
waiting.Enqueue("An");
waiting.Enqueue("Binh");
Console.WriteLine(waiting.Peek());
Console.WriteLine(waiting.Count);
Console.WriteLine(waiting.Dequeue());
Console.WriteLine(waiting.Count);
Console.WriteLine(waiting.Dequeue());
Console.WriteLine(waiting.Count);
```

**Kết quả:**

```text
An
2
An
1
Binh
0
```

Peek trả người đầu hàng nhưng không xóa; Dequeue trả người đầu hàng và xóa khỏi Queue. Thứ tự vào trước, ra trước (FIFO).

#### Ví dụ 3 — Contains, Clear và kiểm tra hàng rỗng

```csharp
Queue<string> waiting = new Queue<string>();
waiting.Enqueue("An");
Console.WriteLine(waiting.Contains("An"));
Console.WriteLine(waiting.Contains("Chi"));
waiting.Clear();
Console.WriteLine(waiting.Count);
if (waiting.Count > 0)
{
    Console.WriteLine(waiting.Dequeue());
}
else
{
    Console.WriteLine("Hang cho rong");
}
```

**Kết quả:**

```text
True
False
0
Hang cho rong
```

Contains trả bool; Clear xóa toàn bộ và trả void. Dequeue/Peek khi rỗng gây InvalidOperationException.

### 5.3. Bảng so sánh

| Nhu cầu | Stack | Queue |
| --- | --- | --- |
| Thêm | Push, trả void | Enqueue, trả void |
| Xem, không xóa | Peek, trả phần tử đỉnh | Peek, trả phần tử đầu hàng |
| Lấy và xóa | Pop, trả phần tử đỉnh | Dequeue, trả phần tử đầu hàng |
| Kiểm tra tồn tại | Contains, trả bool | Contains, trả bool |
| Xóa hết | Clear, trả void | Clear, trả void |
| Đếm | Count, thuộc tính int | Count, thuộc tính int |

---

## 6. HashSet<T> — tập hợp không trùng

HashSet dùng để lưu giá trị duy nhất hoặc kiểm tra đã có mã hay chưa.

Mỗi ví dụ bên dưới **chạy độc lập** trong Main của project Console, có `using System;` và `using System.Collections.Generic;`. Không ghép các ví dụ nguyên trạng vào cùng Main vì có thể trùng tên biến. Khi có khai báo enum/struct/class, đặt khai báo đó **ngoài Main**, cùng cấp với class Program.

Các hàm không tự in ra màn hình; kết quả dưới mỗi ví dụ do Console.WriteLine in giá trị trả về hoặc trạng thái sau thao tác. Count là thuộc tính, không viết Count().

#### Ví dụ 1 — Add và Count — chỉ giữ giá trị duy nhất

```csharp
HashSet<string> achievements = new HashSet<string>();
Console.WriteLine(achievements.Add("first_win"));
Console.WriteLine(achievements.Add("first_win"));
Console.WriteLine(achievements.Add("First_Win"));
Console.WriteLine(achievements.Count);
```

**Kết quả:**

```text
True
False
True
2
```

Khác List.Add trả void, HashSet.Add trả bool: true nếu thêm mới, false nếu đã tồn tại. Chuỗi trong ví dụ phân biệt hoa/thường, nên first_win và First_Win là hai giá trị khác nhau.

#### Ví dụ 2 — Contains và Remove

```csharp
HashSet<string> achievements = new HashSet<string> { "first_win", "collector" };
Console.WriteLine(achievements.Contains("first_win"));
Console.WriteLine(achievements.Remove("first_win"));
Console.WriteLine(achievements.Contains("first_win"));
Console.WriteLine(achievements.Remove("first_win"));
Console.WriteLine(achievements.Count);
```

**Kết quả:**

```text
True
True
False
False
1
```

Contains kiểm tra tồn tại; Remove xóa giá trị nếu có. Cả hai trả bool. Lần Remove thứ hai trả false vì mã đã bị xóa.

#### Ví dụ 3 — foreach và Clear

```csharp
HashSet<string> achievements = new HashSet<string> { "first_win", "collector" };
int visited = 0;
foreach (string code in achievements)
{
    visited++;
}
Console.WriteLine($"So ma da duyet: {visited}");
achievements.Clear();
Console.WriteLine(achievements.Count);
```

**Kết quả:**

```text
So ma da duyet: 2
0
```

HashSet duyệt được bằng foreach, nhưng không truy cập bằng [0] và không dựa vào thứ tự duyệt. Ví dụ chỉ đếm lượt để kết quả không phụ thuộc thứ tự. Clear trả void.

---

## 7. Enum — đặt tên cho trạng thái

Enum tạo một kiểu có các hằng được đặt tên, giúp đọc logic trạng thái dễ hơn các số rời rạc.

Mỗi ví dụ bên dưới **chạy độc lập** trong Main của project Console, có `using System;` và `using System.Collections.Generic;`. Không ghép các ví dụ nguyên trạng vào cùng Main vì có thể trùng tên biến. Khi có khai báo enum/struct/class, đặt khai báo đó **ngoài Main**, cùng cấp với class Program.

Các hàm không tự in ra màn hình; kết quả dưới mỗi ví dụ do Console.WriteLine in giá trị trả về hoặc trạng thái sau thao tác. Count là thuộc tính, không viết Count().

#### Ví dụ 1 — Gán, so sánh và in enum

**Khai báo ngoài Main:**

```csharp
enum GameState
{
    Menu,
    Playing,
    Paused,
    GameOver
}
```

**Đặt trong Main:**

```csharp
GameState state = GameState.Playing;
Console.WriteLine(state);
Console.WriteLine(state == GameState.Playing);
state = GameState.Paused;
Console.WriteLine(state);
Console.WriteLine((int)state);
```

**Kết quả:**

```text
Playing
True
Paused
2
```

Các thành viên không gán số riêng bắt đầu từ 0 và tăng dần: Menu = 0, Playing = 1, Paused = 2, GameOver = 3. Ép sang int ở dòng cuối để quan sát mã số; khi viết logic nên dùng tên trạng thái.

#### Ví dụ 2 — switch — chọn hành vi theo trạng thái

**Khai báo ngoài Main:**

```csharp
enum GameState
{
    Menu,
    Playing,
    Paused,
    GameOver
}
```

**Đặt trong Main:**

```csharp
GameState state = GameState.GameOver;
switch (state)
{
    case GameState.Menu:
        Console.WriteLine("Man hinh chinh");
        break;
    case GameState.Playing:
        Console.WriteLine("Dang choi");
        break;
    case GameState.Paused:
        Console.WriteLine("Tam dung");
        break;
    case GameState.GameOver:
        Console.WriteLine("Ket thuc tran");
        break;
}
```

**Kết quả:**

```text
Ket thuc tran
```

Chỉ nhánh khớp trạng thái hiện tại được chạy. Enum đặt tên cho lựa chọn; việc dừng hoặc kết thúc game vẫn cần code xử lý.

---

## 8. Struct và class — giới thiệu hành vi sao chép

Buổi này quan sát hành vi gán; cách thiết kế class sẽ học ở buổi 4.

Mỗi ví dụ bên dưới **chạy độc lập** trong Main của project Console, có `using System;` và `using System.Collections.Generic;`. Không ghép các ví dụ nguyên trạng vào cùng Main vì có thể trùng tên biến. Khi có khai báo enum/struct/class, đặt khai báo đó **ngoài Main**, cùng cấp với class Program.

Các hàm không tự in ra màn hình; kết quả dưới mỗi ví dụ do Console.WriteLine in giá trị trả về hoặc trạng thái sau thao tác. Count là thuộc tính, không viết Count().

#### Ví dụ 1 — Struct — gán tạo bản sao giá trị

**Khai báo ngoài Main:**

```csharp
struct GridPosition
{
    public int X;
    public int Y;
}
```

**Đặt trong Main:**

```csharp
GridPosition first = new GridPosition();
first.X = 2;
first.Y = 3;
GridPosition second = first;
second.X = 99;
Console.WriteLine($"First: ({first.X}, {first.Y})");
Console.WriteLine($"Second: ({second.X}, {second.Y})");
```

**Kết quả:**

```text
First: (2, 3)
Second: (99, 3)
```

Với struct chứa hai field int ở đây, second nhận bản sao giá trị nên sửa second.X không đổi first.X. Nếu struct chứa field kiểu tham chiếu, phép gán không tự sao chép sâu đối tượng ở field đó.

#### Ví dụ 2 — Class — gán sao chép tham chiếu

**Khai báo ngoài Main:**

```csharp
class PlayerData
{
    public int Hp;
}
```

**Đặt trong Main:**

```csharp
PlayerData first = new PlayerData();
first.Hp = 100;
PlayerData second = first;
second.Hp = 50;
Console.WriteLine($"First HP: {first.Hp}");
Console.WriteLine($"Second HP: {second.Hp}");
```

**Kết quả:**

```text
First HP: 50
Second HP: 50
```

Hai biến giữ tham chiếu tới cùng đối tượng PlayerData. Phép gán second = first không gọi new và không tạo đối tượng độc lập.

#### Ví dụ 3 — Class — hai lần new tạo hai đối tượng

**Khai báo ngoài Main:**

```csharp
class PlayerData
{
    public int Hp;
}
```

**Đặt trong Main:**

```csharp
PlayerData first = new PlayerData();
first.Hp = 100;
PlayerData second = new PlayerData();
second.Hp = first.Hp;
second.Hp = 50;
Console.WriteLine($"First HP: {first.Hp}");
Console.WriteLine($"Second HP: {second.Hp}");
```

**Kết quả:**

```text
First HP: 100
Second HP: 50
```

Hai lần new tạo hai đối tượng riêng. Ví dụ sao chép giá trị field int sang đối tượng mới, không phải cơ chế tự sao chép sâu mọi class. Các field public chỉ phục vụ quan sát; buổi 4 học đóng gói.

| Struct | Class |
| --- | --- |
| Kiểu giá trị | Kiểu tham chiếu |
| Gán sao chép giá trị | Gán sao chép tham chiếu |
| Ví dụ: tọa độ dạng giá trị | Ví dụ: người chơi có trạng thái |

Không ghi nhớ bằng quy tắc “struct luôn trên stack, class luôn trên heap”; tập trung vào hành vi sao chép.

---

## 9. #region — thu gọn code trong IDE

Mỗi ví dụ bên dưới **chạy độc lập** trong Main của project Console, có `using System;` và `using System.Collections.Generic;`. Không ghép các ví dụ nguyên trạng vào cùng Main vì có thể trùng tên biến. Khi có khai báo enum/struct/class, đặt khai báo đó **ngoài Main**, cùng cấp với class Program.

Các hàm không tự in ra màn hình; kết quả dưới mỗi ví dụ do Console.WriteLine in giá trị trả về hoặc trạng thái sau thao tác. Count là thuộc tính, không viết Count().

#### Ví dụ 1 — #region chỉ nhóm phần hiển thị trong IDE

```csharp
#region Inventory
int ammo = 10;
Console.WriteLine($"Trong region: {ammo}");
#endregion
ammo += 5;
Console.WriteLine($"Sau region: {ammo}");
```

**Kết quả:**

```text
Trong region: 10
Sau region: 15
```

#region/#endregion cho phép thu gọn code trong IDE; không tạo phạm vi biến mới. ammo vẫn dùng được sau #endregion. Thu gọn hoặc mở vùng không thay đổi kết quả chạy; region không thay thế việc chia hàm/class.

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
