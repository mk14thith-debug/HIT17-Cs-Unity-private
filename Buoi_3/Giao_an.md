# Giáo án buổi 3 — Chọn cách lưu dữ liệu trong C#

[← Lộ trình](../README.md) · [Lý thuyết](Ly_thuyet.md) · [Bài tập về nhà](BTVN.md)

## 1. Cách dùng giáo án

Giáo án dành cho lớp mới học xong biến, điều kiện, vòng lặp, mảng và hàm. Thời lượng đề xuất là **90 phút**, chưa tính giờ nghỉ. Các đoạn “Lời dẫn” có thể dùng trực tiếp khi giảng; đáp án dành cho người dạy, nên cho học viên dự đoán trước khi chiếu.

**Câu chuyện xuyên suốt:** Viết chương trình Console quản lý dữ liệu cho một game: vật phẩm nhặt được, số lượng trong kho, hàng chờ người chơi và trạng thái trận đấu.

**Đích đến:** Học viên chọn được cấu trúc dữ liệu theo nhu cầu và thực hiện được thao tác cơ bản. Ưu tiên List, Dictionary và enum; những phần còn lại bắt đầu bằng một ví dụ nhỏ.

Sau buổi học, học viên cần:

1. Thêm, đọc, sửa, xóa và duyệt một List.
2. Đếm vật phẩm và tra cứu an toàn bằng Dictionary.
3. Phân biệt thứ tự lấy dữ liệu của Stack và Queue; dùng HashSet để tránh trùng.
4. Dùng enum đặt tên trạng thái; dự đoán được kết quả gán struct và class.
5. Hiểu `var` vẫn có kiểu cố định; nhận biết `object`, `dynamic` và `#region`.

**Chuẩn bị:** Một project C# Console chạy được. Giữ cấu trúc `Program.Main` như tài liệu lớp. Không cần mở Unity.

Với các đoạn ghi “đặt trong Main”, dùng khung sau; mỗi demo chạy riêng để tránh khai báo trùng tên biến:

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Đặt đoạn demo ở đây.
    }
}
```

## 2. Kế hoạch 90 phút

| Mốc | Phút | Nội dung | Dấu hiệu học viên hiểu |
| --- | ---: | --- | --- |
| 00–05 | 5 | Ôn mảng, nêu bài toán | Nhớ chỉ số bắt đầu từ 0, độ dài mảng cố định |
| 05–12 | 7 | var, object, dynamic | Biết var không tự đổi kiểu |
| 12–30 | 18 | List và bài tập nhanh | Thêm, xóa, duyệt đúng |
| 30–48 | 18 | Dictionary và đếm vật phẩm | Biết khóa duy nhất, tra mã không có an toàn |
| 48–60 | 12 | Stack, Queue, HashSet | Chọn đúng cấu trúc qua tình huống |
| 60–66 | 6 | Enum | Thay mã số bằng tên trạng thái |
| 66–75 | 9 | Struct/class và #region | Dự đoán được hành vi sao chép |
| 75–87 | 12 | Thực hành tổng hợp | Đếm đúng và xử lý mã chưa tồn tại |
| 87–90 | 3 | Kiểm tra cuối buổi, giao bài | Giải thích được lý do chọn collection |
| **Tổng** | **90** | | |

Nhịp giảng mỗi phần: **nêu nhu cầu → cho dự đoán → chạy ví dụ → học viên đổi một chi tiết → chốt một câu**. Khi học viên bí, yêu cầu viết dữ liệu hiện tại ra giấy trước khi sửa cú pháp.

## 3. Kịch bản giảng trực tiếp

### 00–05 — Từ mảng đến collection

**Lời dẫn:** “Buổi trước ta đã lưu ba điểm số bằng mảng. Bây giờ nhân vật liên tục nhặt thêm đồ. Mình cần một cách lưu cho phép thêm và xóa thuận tiện.”

Viết:

```csharp
string[] items = new string[3];
```

Hỏi lớp:

- Mảng này có bao nhiêu ô? **3.**
- Chỉ số hợp lệ là gì? **0, 1, 2.**
- Gán `items[3] = "ammo"` có làm mảng dài ra không? **Không, truy cập vượt phạm vi.**

**Chốt:** Mảng có độ dài cố định sau khi tạo. Collection cung cấp nhiều cách quản lý nhóm dữ liệu; ta chọn theo thao tác cần làm.

### 05–12 — var, object, dynamic: hiểu vừa đủ để đọc code

**Lời dẫn:** “Trước khi dùng danh sách, mình làm rõ ba cách khai báo các em sẽ gặp. Câu hỏi là: kiểu của biến được xác định thế nào, và lỗi được phát hiện lúc nào?”

**Demo A — đặt trong Main:**

```csharp
var score = 100;
score = 200;
Console.WriteLine(score); // 200

// score = "HIT"; // Bỏ chú thích sẽ lỗi biên dịch: score có kiểu int.
```

Hỏi: “Máy suy ra `score` là kiểu gì?” **int, từ giá trị 100.**

**Chốt:** `var` nhờ trình biên dịch suy ra kiểu từ giá trị khởi tạo. Kiểu đó vẫn cố định. Không viết được `var score;`.

**Demo B — đặt trong Main:**

```csharp
object data = 100;
int number = (int)data;
Console.WriteLine(number + 1); // 101

data = "HIT";
Console.WriteLine(data); // HIT
```

Giải thích: Biến `object` có thể chứa dữ liệu thuộc nhiều kiểu; muốn dùng như một kiểu cụ thể phải xử lý kiểu phù hợp. Gán int vào object là boxing; lấy lại đúng int là unboxing. Chỉ giới thiệu tên, chưa đi sâu hiệu năng.

**Demo C — đặt trong Main:**

```csharp
dynamic value = "HIT";
Console.WriteLine(value.Length); // 3
value = 100;
// Console.WriteLine(value.Length); // Lỗi lúc chạy nếu thực hiện dòng này.
```

**Chốt:** `dynamic` chuyển một số kiểm tra thao tác sang lúc chạy. Trong bài hôm nay dùng kiểu cụ thể hoặc `var` khi kiểu đã rõ.

**Câu kiểm tra:** “Đổi `int score = 100` thành `var score = 100` có khiến biến chứa được chuỗi không?” **Không.**

### 12–30 — List: danh sách có thể thêm, xóa

**Lời dẫn:** “Hãy hình dung túi đồ là một danh sách các lượt nhặt. Nhặt hai viên đạn thì danh sách có thể chứa hai lần ammo.”

Giải thích `List<string>`: List là loại danh sách; `string` cho biết kiểu của mỗi phần tử. Chưa cần mở rộng lý thuyết generic.

**Demo — đặt trong Main, chạy lần lượt từng đoạn:**

```csharp
List<string> bag = new List<string>();
bag.Add("ammo");
bag.Add("shield");
bag.Add("ammo");

Console.WriteLine(bag.Count); // 3
Console.WriteLine(bag[0]);    // ammo
bag[1] = "potion";

bool removed = bag.Remove("ammo");
Console.WriteLine(removed);  // True

foreach (string item in bag)
{
    Console.WriteLine(item); // potion, rồi ammo trên hai dòng
}
```

Vẽ trạng thái lên bảng:

```text
Sau ba lần Add:    [ammo] [shield] [ammo]
Chỉ số:              0       1       2
Sau bag[1] = ...:  [ammo] [potion] [ammo]
Sau Remove:        [potion] [ammo]
Chỉ số mới:            0       1
```

Nhấn mạnh chỉ số thay đổi sau khi xóa. `Remove("ammo")` xóa **lần xuất hiện đầu tiên**, trả bool cho biết có xóa được không.

| Cần làm | Cú pháp |
| --- | --- |
| Thêm cuối | `bag.Add("ammo")` |
| Đếm phần tử | `bag.Count` |
| Kiểm tra tồn tại | `bag.Contains("ammo")` |
| Xóa theo giá trị | `bag.Remove("ammo")` |
| Xóa theo chỉ số | `bag.RemoveAt(0)` |

`RemoveAt` cần chỉ số hợp lệ. List rỗng không có phần tử ở chỉ số 0. Mảng dùng `Length`; List dùng `Count`.

#### Ví dụ cụ thể để chọn khi giảng

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

**Bài tại lớp — 4 phút:** Tạo `List<string>` gồm An, Bình, Chi. Thêm Dũng, xóa Bình, in số người và tên còn lại bằng foreach.

**Kết quả:** 3 người, theo thứ tự An, Chi, Dũng.

**Câu kiểm tra:** “Nếu danh sách có hai An, Remove một lần sẽ còn mấy An?” **Một.**

**Lưu ý:** Không thêm/xóa List trong lúc foreach đang duyệt chính List đó. Nếu cần sửa danh sách, kết thúc lượt duyệt rồi thực hiện thao tác. `Find` chỉ giới thiệu thêm nếu lớp theo kịp; cú pháp lambda sẽ học ở buổi 5.

### 30–48 — Dictionary: tìm dữ liệu bằng khóa

**Lời dẫn:** “List cho biết từng lượt nhặt. Nhưng khi hỏi ‘mình có bao nhiêu ammo?’, ta muốn tra theo mã ammo. Dictionary lưu mỗi mã cùng số lượng tương ứng.”

Vẽ:

| Khóa | Giá trị |
| --- | ---: |
| ammo | 3 |
| shield | 2 |
| potion | 1 |

**Demo A — đặt trong Main:**

```csharp
Dictionary<string, int> inventory = new Dictionary<string, int>();
inventory.Add("ammo", 3);
inventory.Add("shield", 2);

inventory["ammo"] = 5;    // Cập nhật khóa đã có.
inventory["potion"] = 1;  // Thêm khóa chưa có.

Console.WriteLine(inventory["ammo"]); // 5

if (inventory.TryGetValue("laser", out int quantity))
{
    Console.WriteLine(quantity);
}
else
{
    Console.WriteLine("Khong co laser");
}
```

Giải thích `TryGetValue`: “Thử tìm mã này. Nếu có, trả true và đưa số lượng vào quantity; nếu không, đi vào else.” Với int, quantity nhận 0 khi tìm không thấy. `out` sẽ học kỹ ở buổi 4.

**Ba điểm phải chốt:**

1. Mỗi khóa chỉ xuất hiện một lần; nhiều khóa có thể có cùng giá trị.
2. `Add` với khóa đã tồn tại gây lỗi; `inventory[key] = value` thêm hoặc cập nhật.
3. Đọc `inventory["laser"]` khi không có khóa gây lỗi. Dùng TryGetValue hoặc kiểm tra ContainsKey.

**Demo B — đếm từng lượt nhặt, đặt trong Main độc lập với demo A:**

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

Console.WriteLine(counts["ammo"]);   // 2
Console.WriteLine(counts["shield"]); // 1
```

Đi chậm qua từng vòng:

| Lượt | item | Đã có khóa? | Sau xử lý |
| --- | --- | --- | --- |
| 1 | ammo | Chưa | ammo: 1 |
| 2 | shield | Chưa | ammo: 1, shield: 1 |
| 3 | ammo | Có | ammo: 2, shield: 1 |

Các ví dụ bổ sung là kho demo để chọn theo thời gian; giữ nguyên mốc của giáo án và thời gian thực hành, không yêu cầu chạy toàn bộ.

#### Ví dụ cụ thể và kết quả để chọn khi giảng

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

**Bài tại lớp — 4 phút:** Thêm potion và shield vào dữ liệu đầu vào rồi đếm lại. **Kết quả:** ammo 2, shield 2, potion 1.

**Câu kiểm tra:** “`counts.Count` bằng 3 có nghĩa đã nhặt ba món không?” **Không, đó là ba khóa, tương ứng ba loại vật phẩm; tổng số lượt nhặt là 5.**

### 48–60 — Stack, Queue, HashSet: ba nhu cầu khác nhau

Trước khi viết code, dùng ba tình huống đời thường:

- **Stack — chồng đĩa:** Đặt sau lấy trước. Dùng cho lịch sử thao tác.
- **Queue — xếp hàng:** Đến trước ra trước. Dùng cho hàng chờ vào trận.
- **HashSet — tập mã thành tựu:** Thêm lại mã đã có vẫn chỉ tính một lần.

**Demo — đặt trong Main:**

```csharp
Stack<string> history = new Stack<string>();
history.Push("Di chuyen");
history.Push("Ban");
Console.WriteLine(history.Peek()); // Ban; chưa xóa.
Console.WriteLine(history.Pop());  // Ban; đã xóa.
Console.WriteLine(history.Count);  // 1

Queue<string> waiting = new Queue<string>();
waiting.Enqueue("An");
waiting.Enqueue("Binh");
Console.WriteLine(waiting.Dequeue()); // An
Console.WriteLine(waiting.Peek());    // Binh; chưa xóa.

HashSet<string> achievements = new HashSet<string>();
Console.WriteLine(achievements.Add("first_win")); // True
Console.WriteLine(achievements.Add("first_win")); // False
Console.WriteLine(achievements.Count);            // 1
```

**Nhắc ngay sau demo:** Pop, Dequeue và Peek gây lỗi khi rỗng. Kiểm tra `Count > 0` trước khi gọi. Stack chỉ lưu dữ liệu theo thứ tự; muốn undo thật sự phải viết thêm hành vi hoàn tác.

HashSet có `Contains`, `Remove`; không truy cập bằng `[0]` và không dựa vào thứ tự duyệt. Với chuỗi trong các ví dụ này, mặc định có phân biệt hoa/thường.

**Hỏi nhanh cả lớp:**

| Tình huống | Đáp án |
| --- | --- |
| Lấy thao tác gần nhất để hoàn tác | Stack |
| Gọi người đến sớm nhất | Queue |
| Đếm mã thành tựu duy nhất | HashSet |
| Lưu mọi lượt nhặt, gồm cả trùng | List |
| Tra số lượng theo mã vật phẩm | Dictionary |

Các ví dụ bổ sung là kho demo để chọn theo thời gian; giữ nguyên mốc của giáo án và thời gian thực hành, không yêu cầu chạy toàn bộ.

#### Ví dụ riêng cho từng thao tác Stack

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

#### Ví dụ riêng cho từng thao tác Queue

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

#### Ví dụ riêng cho từng thao tác HashSet

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

### 60–66 — Enum: gọi trạng thái bằng tên

**Lời dẫn:** “Nếu chỉ nhìn `state = 2`, người đọc chưa biết 2 là đang chơi hay tạm dừng. Mình đặt tên cho các lựa chọn bằng enum.”

**Đặt ngoài Main, phía trên class Program:**

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

if (state == GameState.Playing)
{
    Console.WriteLine("Dang choi");
}

state = GameState.Paused;
Console.WriteLine(state); // Paused
```

Các ví dụ bổ sung là kho demo để chọn theo thời gian; giữ nguyên mốc của giáo án và thời gian thực hành, không yêu cầu chạy toàn bộ.

#### Ví dụ cụ thể và kết quả để chọn khi giảng

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

**Bài nhanh:** Đổi trạng thái thành GameOver, dùng if hoặc switch để in “Ket thuc tran”.

**Chốt:** Enum tạo một kiểu có các hằng được đặt tên, giúp code trạng thái dễ đọc. Việc chuyển trạng thái vẫn cần lập trình; enum không tự tạm dừng game.

### 66–75 — Struct/class: gán xong thì dữ liệu có độc lập không?

**Lời dẫn:** “Cùng viết `second = first`, nhưng có lúc ta sao chép giá trị, có lúc ta sao chép tham chiếu. Mình chạy hai ví dụ để nhìn thấy khác biệt.”

**Demo đầy đủ — thay toàn bộ Program.cs bằng đoạn sau:**

```csharp
using System;

struct GridPosition
{
    public int X;
    public int Y;
}

class PlayerData
{
    public int Hp;
}

class Program
{
    static void Main()
    {
        GridPosition original = new GridPosition();
        original.X = 2;
        original.Y = 3;
        GridPosition copy = original;
        copy.X = 99;

        Console.WriteLine($"Original: ({original.X}, {original.Y})");
        Console.WriteLine($"Copy: ({copy.X}, {copy.Y})");

        PlayerData first = new PlayerData();
        first.Hp = 100;
        PlayerData second = first;
        second.Hp = 50;

        Console.WriteLine($"First HP: {first.Hp}");
        Console.WriteLine($"Second HP: {second.Hp}");
    }
}
```

**Cho lớp dự đoán trước khi chạy. Kết quả:**

```text
Original: (2, 3)
Copy: (99, 3)
First HP: 50
Second HP: 50
```

Vẽ lên bảng:

```text
Struct trong ví dụ:
original -> giá trị (2, 3)
copy     -> giá trị (99, 3)

Class trong ví dụ:
first  ----+
           +----> cùng đối tượng PlayerData, Hp = 50
second ----+
```

**Chốt:** Struct là kiểu giá trị, phép gán sao chép giá trị. Class là kiểu tham chiếu, phép gán sao chép tham chiếu. Không cần dùng quy tắc vị trí bộ nhớ để giải thích.

Phạm vi ví dụ struct là hai field int. Nếu struct chứa field kiểu tham chiếu, bản sao vẫn có thể tham chiếu chung đối tượng ở field đó; phép gán không tự sao chép sâu mọi dữ liệu bên trong.

**Hỏi thêm:** “Muốn hai PlayerData độc lập thì làm sao?” **Tạo hai đối tượng bằng hai lần new và gán dữ liệu cần thiết.** Constructor và thiết kế class sẽ học ở buổi 4. Field public ở đây phục vụ quan sát.

**Giới thiệu #region trong 1 phút:** Bọc một phần code bằng `#region TenNhom` và `#endregion`, rồi thu gọn trong IDE. Nó giúp tổ chức hiển thị, không đổi cách chạy và không tạo phạm vi biến mới.

Các ví dụ bổ sung là kho demo để chọn theo thời gian; giữ nguyên mốc của giáo án và thời gian thực hành, không yêu cầu chạy toàn bộ.

#### Ví dụ bổ sung để đối chiếu struct/class

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

#### Ví dụ quan sát #region

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

### 75–87 — Thực hành tổng hợp: chọn một trong hai bài

Viết chương trình C# Console, dùng dữ liệu cố định, không cần menu hoặc nhập bàn phím. Mỗi bài là một project riêng; không dùng LINQ. Phần này chỉ có đề bài, kết quả mẫu và gợi ý, không có mã lời giải.

#### Bài 1 — Kiểm tra túi vật phẩm

**Kiến thức:** List, Dictionary, HashSet, vòng lặp và điều kiện. **Thời gian gợi ý:** 10–12 phút.

**Dữ liệu ban đầu — đặt trong Main:**

```csharp
List<string> bag = new List<string> { "ammo", "shield", "ammo", "potion" };
```

**Yêu cầu:**

1. Thêm một `shield` vào cuối túi, sau đó xóa một `potion` theo giá trị.
2. In các vật phẩm còn lại theo thứ tự trong List và tổng số món.
3. Từ List sau khi thay đổi, dùng Dictionary đếm số lượng từng mã. In số lượng `ammo`, `shield`, `potion` theo thứ tự này; mã không còn trong túi thì in 0.
4. Dùng HashSet chứa các mã trong túi sau khi thay đổi, rồi in số loại vật phẩm khác nhau.
5. Kiểm tra trong túi có `laser` hay không; nếu không có, in `Khong co laser`.

**Kết quả mong đợi:**

```text
Tui: ammo, shield, ammo, shield
So mon: 4
ammo: 2
shield: 2
potion: 0
So loai: 2
Khong co laser
```

**Gợi ý:**

- Hoàn thành thao tác thêm/xóa trước khi bắt đầu thống kê để chỉ cần đếm một lần.
- Phân biệt xóa theo giá trị với xóa theo chỉ số; chọn hàm phù hợp để xóa potion.
- Khi duyệt túi, tự hỏi mã hiện tại đã có trong Dictionary chưa: nếu có thì tăng số lượng, nếu chưa thì bắt đầu từ 1.
- Dùng một danh sách hoặc mảng mã riêng để quyết định thứ tự in. Khi tra mã có thể vắng mặt, nhớ cách lấy giá trị an toàn.
- HashSet chỉ giữ mỗi mã một lần; phân biệt số món của túi với số loại trong tập hợp.
- Có thể dùng foreach hoặc string.Join để in dòng túi đồ; không cần sắp xếp lại List.

**Tự kiểm tra:**

- Trong List ban đầu, đổi `potion` thành `ammo`: chương trình vẫn chạy khi không có potion để xóa; sau các thao tác, ammo có 3, shield có 2, số món là 5 và số loại là 2.
- Giải thích vì sao hai lần xuất hiện của ammo làm số món tăng nhưng chỉ đóng góp một loại.

#### Bài 2 — Hàng chờ vào trận

**Kiến thức:** Queue, Stack, enum, vòng lặp và điều kiện. **Thời gian gợi ý:** 10–12 phút.

**Bối cảnh:** Ba người An, Binh, Chi đến theo thứ tự đó. Hệ thống phục vụ hai người đầu và lưu tên người đã phục vụ vào lịch sử.

**Yêu cầu:**

1. Khai báo enum `MatchState` gồm `Waiting` và `Playing`, đặt ngoài Main. Trạng thái ban đầu là Waiting.
2. Tạo Queue chứa An, Binh, Chi theo đúng thứ tự; tạo Stack rỗng để lưu lịch sử phục vụ. In trạng thái và số người chờ ban đầu.
3. Dùng vòng lặp phục vụ tối đa hai người. Mỗi lượt lấy người đầu Queue, in tên và đưa tên đó vào Stack. Dừng nếu Queue hết người.
4. Nếu đã phục vụ ít nhất một người thì đổi trạng thái thành Playing; nếu chưa có ai thì giữ Waiting. In trạng thái và số người chờ còn lại.
5. Xem người tiếp theo trong Queue và người được phục vụ gần nhất trong Stack, **không xóa họ khỏi các collection**. Nếu collection tương ứng rỗng, in `Hang cho rong` hoặc `Chua phuc vu ai`.
6. In số mục lịch sử để xác nhận thao tác xem không xóa dữ liệu.

**Kết quả mong đợi với ba người ban đầu:**

```text
Trang thai ban dau: Waiting
So nguoi cho ban dau: 3
Phuc vu: An
Phuc vu: Binh
Trang thai: Playing
Con cho: 1
Tiep theo: Chi
Gan nhat: Binh
So muc lich su: 2
```

**Gợi ý:**

- Queue quyết định ai được phục vụ trước; Stack giúp xem ai được phục vụ gần nhất.
- Phân biệt hàm lấy rồi xóa với hàm chỉ xem. Trước khi lấy hoặc xem, kiểm tra collection có phần tử hay không.
- Vòng lặp phải có cả giới hạn hai lượt và điều kiện còn người chờ.
- Dùng số lượt đã phục vụ hoặc số mục lịch sử để quyết định có chuyển trạng thái hay không.
- In trực tiếp biến enum để quan sát tên trạng thái, không cần tự gán các số 0, 1 trong logic.

**Tự kiểm tra:**

- Chỉ có An trong hàng: phục vụ đúng một người, trạng thái Playing, hàng chờ rỗng và lịch sử có một mục.
- Hàng ban đầu rỗng: không phục vụ ai, giữ Waiting; in hai thông báo rỗng tương ứng và số mục lịch sử bằng 0, không phát sinh lỗi.
- Sau khi xem người tiếp theo và người gần nhất, số người chờ và số mục lịch sử không đổi.

#### Cách chọn bài trong thời gian cuối buổi

Hai bài là hai lựa chọn độc lập. Với khung 12 phút, chọn **một bài**: 1 phút đọc đề, 9 phút tự làm, 2 phút đối chiếu kết quả và giải thích cách chọn collection. Học viên hoàn thành sớm có thể bắt đầu bài còn lại; nếu muốn cả lớp làm đủ hai bài, dành khoảng 20–25 phút hoặc tiếp tục ở buổi sau.

Người dạy hỗ trợ bằng câu hỏi gợi mở và gợi ý trong đề; không chiếu hoặc phát mã lời giải. Khi nhận xét, kiểm tra kết quả, thứ tự xử lý, điều kiện rỗng và việc chọn đúng collection.

### 87–90 — Phiếu cuối buổi và giao bài

Yêu cầu học viên trả lời ngắn, chưa xem tài liệu:

1. Muốn lưu tất cả lượt nhặt, kể cả trùng, dùng gì? **List.**
2. Muốn tra số lượng theo mã, dùng gì? **Dictionary.**
3. Stack lấy phần tử mới nhất hay cũ nhất? **Mới nhất.**
4. Thêm một mã hai lần vào HashSet được mấy phần tử? **Một.**
5. Gán class sang biến khác có tự tạo object mới không? **Không.**

Giao [3 bài BTVN](BTVN.md) theo đúng đề hiện có:

- **Bài 1 — Túi vật phẩm:** List lưu lượt nhặt, Dictionary lưu số lượng. Khi xóa phải cập nhật cả hai; số lượng về 0 thì xóa khóa.
- **Bài 2 — Hàng chờ và lịch sử phục vụ:** Queue, Stack và enum; kiểm tra rỗng, đọc kỹ quy tắc sau CLOSE. LAST chỉ xem lịch sử, không xóa.
- **Bài 3 — Thành tựu và vị trí:** HashSet đếm mã duy nhất; struct để quan sát sao chép, giải thích kết quả trong README.

Mỗi bài là một Console project riêng, có `.csproj`, mã nguồn và README ngắn về cách chạy/kết quả tự kiểm tra. Không nộp `bin/`, `obj/`.

## 4. Lỗi thường gặp và cách gợi mở

| Dấu hiệu | Câu gợi ý hoặc cách sửa |
| --- | --- |
| Dùng `list.Length` | “Mảng dùng Length; List dùng thuộc tính nào?” → Count |
| Viết `i <= list.Count` | “Nếu Count là 3, chỉ số cuối là mấy?” → 2; dùng `<` |
| Nhầm Remove với RemoveAt | “Em đang chỉ tên món đồ hay vị trí của nó?” |
| Cho rằng Remove xóa hết mã trùng | Cho chạy danh sách có hai ammo và quan sát |
| Add Dictionary bị trùng khóa | Tách nhánh thêm mới và tăng số lượng |
| Tra khóa không tồn tại bị lỗi | Dùng TryGetValue hoặc ContainsKey trước khi đọc |
| Nhầm số loại với số lượt nhặt | Viết ba ammo ra giấy: 3 lượt, 1 loại |
| Pop/Dequeue/Peek khi rỗng | Kiểm tra Count trước khi lấy/xem |
| Truy cập HashSet bằng chỉ số | Nhắc HashSet là tập giá trị duy nhất, không có chỉ số như List |
| Cho rằng var đổi kiểu được | Bật dòng gán chuỗi vào biến int đã suy ra, quan sát lỗi rồi bỏ dòng đó |
| Đặt enum/struct/class trong Main | Chỉ lại khung file; đặt khai báo kiểu ngoài Main |
| Cho rằng gán class tạo bản sao độc lập | Vẽ hai mũi tên cùng đến một object |

## 5. Điều chỉnh thời lượng

**Nếu chỉ có 60 phút:** Mở bài 3; var/object/dynamic 4; List 12; Dictionary 15; Stack/Queue/HashSet 7; enum 4; struct/class/#region 5; thực hành 8; chốt và giao bài 2. Dùng demo chuẩn bị sẵn, chuyển bài sửa code nhỏ sang tự học. Giữ việc tự viết đếm vật phẩm làm thực hành chính.

**Nếu có 120 phút:** Giữ khung 90 phút, thêm 10 phút viết menu thêm/xóa List; 10 phút thực hành đồng bộ List với bộ đếm Dictionary; 10 phút kiểm tra chéo trường hợp rỗng, mã không có và dữ liệu trùng.

**Nếu lớp chưa theo kịp:** Ưu tiên làm chắc List và Dictionary, cho chạy demo các phần còn lại và dùng 5 phút đầu buổi sau để kiểm tra lại. Chưa yêu cầu học viên tự viết lambda hay giải thích chi tiết boxing/unboxing.

## 6. Bảng chốt để giữ trên màn hình

| Nhu cầu | Chọn |
| --- | --- |
| Số ô cố định, truy cập bằng chỉ số | Mảng |
| Danh sách thêm/xóa, giữ thứ tự và cho phép trùng | List |
| Tra giá trị theo khóa duy nhất | Dictionary |
| Vào sau, ra trước | Stack |
| Vào trước, ra trước | Queue |
| Giá trị duy nhất, kiểm tra trùng | HashSet |
| Nhóm lựa chọn có tên, như trạng thái trận đấu | Enum |

**Lời kết có thể dùng:** “Trước khi viết code, hãy hỏi mình muốn làm gì với dữ liệu: giữ mọi lượt, đếm theo mã, xếp hàng hay bỏ trùng. Nhu cầu đó sẽ giúp mình chọn cấu trúc phù hợp.”
