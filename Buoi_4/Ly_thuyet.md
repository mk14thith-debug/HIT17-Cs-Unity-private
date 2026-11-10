# Lập trình hướng đối tượng trong C#

[← Lý thuyết buổi 3](../Buoi_3/Ly_thuyet.md) · [Thực hành](#thực-hành-tổng-hợp)

## Giới thiệu về OOP

**OOP** là viết tắt của *Object-Oriented Programming*, nghĩa là **lập trình hướng đối tượng**. Đây là cách tổ chức chương trình quanh các **đối tượng**. Mỗi đối tượng giữ dữ liệu của mình và có những thao tác liên quan đến dữ liệu đó. Trong C#, **class** mô tả dữ liệu và thao tác của một loại đối tượng.

Ví dụ trong **Tank Battle**, class `Tank` mô tả một xe tăng có tên, HP và thao tác nhận sát thương. Xe `Armor` và xe `Swift` là hai đối tượng cụ thể. Khi `Armor` trúng đạn, chương trình gọi `armor.TakeDamage(20)` để xe đó xử lý sát thương. HP của `Swift` vẫn giữ nguyên vì mỗi xe có trạng thái riêng.

Khi chương trình lớn dần, OOP giúp ta:

- **Chia trách nhiệm rõ ràng.** Xe tăng quản lý HP, còn vũ khí quản lý cách bắn và số đạn.
- **Đặt quy tắc ở một nơi.** Ví dụ, `Tank` giữ HP từ 0 trở lên mỗi khi nhận sát thương. Va chạm và đạn bắn đều có thể gọi cùng một thao tác.
- **Dùng lại phần chung và mở rộng hành vi.** Xe hạng nặng và xe trinh sát có thể chia sẻ cách mô tả, đồng thời có cách bắn riêng. Phần xử lý lượt có thể yêu cầu từng xe bắn qua cùng một lời gọi.

Trong buổi này, ta sẽ tạo xe trước, rồi dùng các tình huống trên để hiểu **đóng gói, kế thừa, trừu tượng và đa hình**.

Mỗi mục có lý thuyết, ví dụ và kết quả để tự đối chiếu. Thay toàn bộ `Program.cs` khi chạy ví dụ mới. Bật nullable reference types ở mục 12.

## 1. Class, đối tượng, new và constructor

**Lý thuyết chung**

**Class** là kiểu do người lập trình định nghĩa. Nó mô tả dữ liệu một đối tượng có thể lưu và các thao tác đối tượng có thể thực hiện. **Object** là một đối tượng cụ thể thuộc kiểu đó. Mỗi đối tượng có thể có trạng thái riêng.

- `TenLop bien;` khai báo một biến kiểu class.
- `new TenLop(...)` tạo đối tượng và gọi **constructor** để khởi tạo dữ liệu. Constructor trùng tên class, có thể nhận tham số và không khai báo kiểu trả về.
- Một biến kiểu class giữ **tham chiếu** tới đối tượng. Hai lần `new` tạo hai đối tượng. Gán một biến kiểu class cho biến khác sao chép tham chiếu, nên cả hai có thể cùng truy cập một đối tượng.

**Ví dụ 1 — Tạo hai xe, đổi HP của một xe**

```csharp
using System;

class Tank
{
    public string Name;
    public int Hp;

    public Tank(string name, int hp)
    {
        Name = name;
        Hp = hp;
    }
}

class Program
{
    static void Main()
    {
        Tank armor = new Tank("Armor", 100);
        Tank swift = new Tank("Swift", 100);

        armor.Hp = 70;

        Console.WriteLine($"{armor.Name}: {armor.Hp}");
        Console.WriteLine($"{swift.Name}: {swift.Hp}");
    }
}
```

**Kết quả mong đợi**

```text
Armor: 70
Swift: 100
```

**Phân tích ví dụ**

`Tank` là class. Hai lời gọi `new Tank(...)` tạo hai xe có tên và HP riêng. Khi gán `armor.Hp = 70`, chỉ đối tượng mà `armor` tham chiếu bị thay đổi. Xe `swift` vẫn có 100 HP.

**Kiểm tra nhanh.** Khoanh hai dòng `new`. Sau đó thay dòng tạo `swift` bằng `Tank swift = armor;`. Hai biến lúc này cùng tham chiếu đến một xe. Dự đoán cả hai dòng in.

## 2. Phương thức instance: giao việc cho từng đối tượng

**Lý thuyết chung**

**Phương thức instance** là phương thức được gọi trên một đối tượng cụ thể. Cú pháp gọi là `doiTuong.PhuongThuc(doiSo)`. Đối số cung cấp giá trị cho tham số, còn đối tượng trước dấu chấm xác định phương thức đang làm việc với trạng thái nào.

Bên trong phương thức instance, ta có thể đọc hoặc cập nhật field và property của đối tượng hiện tại. Cùng một phương thức có thể chạy trên nhiều đối tượng khác nhau của cùng class.

**Ví dụ 2 — Chỉ xe nhận lời gọi bị trừ HP**

```csharp
using System;

class Tank
{
    public int Hp = 100;

    public void TakeDamage(int damage)
    {
        Hp -= damage;
    }
}

class Program
{
    static void Main()
    {
        Tank armor = new Tank();
        Tank swift = new Tank();

        armor.TakeDamage(20);

        Console.WriteLine($"Armor: {armor.Hp}");
        Console.WriteLine($"Swift: {swift.Hp}");
    }
}
```

**Kết quả mong đợi**

```text
Armor: 80
Swift: 100
```

**Phân tích ví dụ**

Trong `armor.TakeDamage(20)`, tham số `damage` nhận 20 và dòng `Hp -= damage` dùng HP của `armor`. Xe `swift` là đối tượng riêng nên vẫn có 100 HP. Đoạn code này đang trừ trực tiếp. Mục 5 sẽ bổ sung quy tắc cho trường hợp sát thương vượt quá HP còn lại.

**Kiểm tra nhanh.** Nếu `armor` đang có 80 HP và nhận tiếp 100 sát thương, công thức hiện tại cho ra bao nhiêu? Giữ câu hỏi này để mở đầu phần đóng gói.

## 3. Field, property và expression body

### 3.1. Chỗ lưu dữ liệu và cách truy cập

**Lý thuyết chung**

**Field** là biến được khai báo trong một kiểu, dùng để lưu dữ liệu. Field instance lưu dữ liệu riêng cho từng đối tượng. Khai báo `private` giới hạn việc truy cập trực tiếp field trong kiểu chứa nó.

**Property** là thành viên mà bên gọi đọc hoặc gán bằng cú pháp giống field. Khi đọc `tank.Hp`, C# gọi getter (`get`). Khi gán `tank.Name = "Armor"`, C# gọi setter (`set`). Setter nhận giá trị ở vế phải và quyết định cách cập nhật dữ liệu.

| Dạng property công khai | Bên ngoài class | Cách lưu hoặc lấy giá trị | Khi áp dụng |
| --- | --- | --- | --- |
| `{ get; set; }` | Đọc và gán được | Trình biên dịch tạo field ẩn để lưu giá trị. | Dữ liệu mà bên gọi được phép thay đổi trực tiếp, như `Name` hoặc tùy chọn giao diện. |
| `{ get { return ...; } }` | Chỉ đọc được | Getter tự viết đọc một field hoặc tính giá trị mỗi lần đọc. | Cho đọc field riêng như `_hp` hoặc tính trạng thái như `IsDestroyed`. |
| `{ get; private set; }` | Chỉ đọc được | Trình biên dịch tạo field ẩn. Class có thể cập nhật giá trị qua setter. | Trạng thái được cập nhật qua phương thức của class, như `Hp` qua `TakeDamage()` hoặc điểm số qua `AddScore()`. |

**Ví dụ 3.1 — Đổi tên qua setter, cập nhật HP qua phương thức**

```csharp
using System;

class Tank
{
    private int _hp = 100;

    public string Name { get; set; } = "Tank";

    public int Hp
    {
        get { return _hp; }
    }

    public bool IsDestroyed
    {
        get { return Hp == 0; }
    }

    public void TakeDamage(int damage)
    {
        if (damage > 0)
        {
            _hp = Math.Max(0, _hp - damage);
        }
    }
}

class Program
{
    static void Main()
    {
        Tank tank = new Tank();
        tank.Name = "Armor";
        Console.WriteLine($"{tank.Name}: {tank.Hp}");

        tank.TakeDamage(30);
        Console.WriteLine($"{tank.Name}: {tank.Hp}");
        Console.WriteLine(tank.IsDestroyed);

        tank.TakeDamage(70);
        Console.WriteLine(tank.IsDestroyed);
    }
}
```

**Kết quả mong đợi**

```text
Armor: 100
Armor: 70
False
True
```

**Phân tích ví dụ**

- `tank.Name = "Armor"` gọi setter của auto-property `Name`. Lần đọc tiếp theo gọi getter và nhận tên mới.
- `TakeDamage(30)` cập nhật field `_hp` từ 100 xuống 70. Lần đọc `tank.Hp` tiếp theo trả về 70. Mỗi đối tượng `Tank` có một `_hp` riêng.
- Getter của `IsDestroyed` tính `Hp == 0` mỗi lần đọc. Kết quả là `False` khi xe còn 70 HP và `True` sau khi nhận thêm 70 sát thương.

**Biến thể của ví dụ 3.1 — Dùng setter riêng cho HP**

Trong class `Tank` ở trên, bỏ field `_hp` và thay property `Hp` cùng phương thức `TakeDamage()` bằng đoạn sau. Giữ các phần còn lại của ví dụ để chạy và so sánh kết quả.

```csharp
public int Hp { get; private set; } = 100;

public void TakeDamage(int damage)
{
    if (damage > 0)
    {
        Hp = Math.Max(0, Hp - damage);
    }
}
```

Trong cả hai phiên bản, `Main()` đọc được `tank.Hp` và yêu cầu thay đổi HP qua `TakeDamage()`. Với getter tự viết, `TakeDamage()` gán vào field `_hp`. Với `private set`, phương thức gán vào `Hp` và setter lưu giá trị vào field ẩn.

**Kiểm tra nhanh.** Thử thêm `tank.Hp = 99;` trong `Main()` với mỗi phiên bản. Phiên bản đầu có getter duy nhất, còn phiên bản sau có setter riêng trong `Tank`. Cả hai đều báo lỗi biên dịch tại dòng gán trong `Main()`. Xóa dòng đó để chạy lại ví dụ.

### 3.2. Rút gọn getter sau khi hiểu cách đọc field

**Lý thuyết chung**

**Expression body** dùng `=>` để viết thân thành viên bằng một biểu thức. Với property chỉ đọc, `public int Value => _value;` tương đương `public int Value { get { return _value; } }`. Biểu thức được đánh giá mỗi lần đọc property. Dùng thân có dấu ngoặc khi getter cần nhiều câu lệnh.

**Ví dụ 3.2 — Property đọc field mới sau khi cập nhật HP**

```csharp
using System;

class Tank
{
    private int _hp = 100;

    public int HP => _hp;

    public void TakeDamage(int damage)
    {
        if (damage > 0)
        {
            _hp = Math.Max(0, _hp - damage);
        }
    }
}

class Program
{
    static void Main()
    {
        Tank tank = new Tank();
        Console.WriteLine($"Ban dau: {tank.HP}");

        tank.TakeDamage(25);
        Console.WriteLine($"Sau trung dan: {tank.HP}");
    }
}
```

**Kết quả mong đợi**

```text
Ban dau: 100
Sau trung dan: 75
```

**Phân tích ví dụ**

`public int HP => _hp;` trả về giá trị hiện tại của `_hp`. Trước khi nhận sát thương, getter trả 100. Sau `TakeDamage(25)`, getter đọc lại field và trả 75.

**Kiểm tra nhanh.** Đổi property sang getter có `return` và kiểm tra hai kết quả vẫn giống nhau.

## 4. this: tham chiếu đến đối tượng hiện tại

**Lý thuyết chung**

`this` là tham chiếu tới **đối tượng hiện tại** trong constructor hoặc phương thức instance. Khi field và tham số trùng tên, có thể viết `this.ten = ten;`. Vế trái chỉ thành viên của đối tượng hiện tại, còn vế phải dùng tham số đang có trong phạm vi. Khi không trùng tên, nhiều truy cập thành viên có thể viết gọn mà bỏ `this`.

**Ví dụ 4 — Hai lần gọi Describe có hai đối tượng hiện tại**

```csharp
using System;

class Tank
{
    private string name;

    public Tank(string name)
    {
        this.name = name;
    }

    public string Describe()
    {
        return $"Xe: {this.name}";
    }
}

class Program
{
    static void Main()
    {
        Tank armor = new Tank("Armor");
        Tank swift = new Tank("Swift");
        Console.WriteLine(armor.Describe());
        Console.WriteLine(swift.Describe());
    }
}
```

**Kết quả mong đợi**

```text
Xe: Armor
Xe: Swift
```

**Phân tích ví dụ**

Trong mỗi lần gọi constructor, `this.name` thuộc xe đang được tạo. Khi gọi `armor.Describe()`, `this` tham chiếu đến đối tượng mà `armor` đang giữ. Lời gọi `swift.Describe()` chạy cùng đoạn code trên một đối tượng khác nên đọc tên `Swift`.

**Biến thể tương đương không dùng `this`**

Giữ nguyên `Program.Main` và thay class `Tank` bằng phiên bản dưới đây.

```csharp
class Tank
{
    private string _name;

    public Tank(string name)
    {
        _name = name;
    }

    public string Describe()
    {
        return $"Xe: {_name}";
    }
}
```

**Kết quả**

```text
Xe: Armor
Xe: Swift
```

Ở bản đầu, field và tham số đều tên `name`, nên `this.name = name;` chỉ rõ vế trái là field. Ở bản này, `_name` là field và `name` là tham số. Hai tên đã khác nhau, nên `_name = name;` đã đủ rõ. Trong `Describe()`, `_name` vẫn là field của xe đang nhận lời gọi.

**Kiểm tra nhanh.** Trong `_name = name;`, đâu là field và đâu là tham số constructor?

## 5. Đóng gói (Encapsulation) — Đối tượng tự giữ HP hợp lệ

**Lý thuyết chung**

**Đóng gói** là tổ chức trạng thái cùng các thao tác chịu trách nhiệm cập nhật trạng thái đó. Kiểu quyết định dữ liệu nào được đọc, thay đổi qua thao tác nào và điều kiện nào luôn phải được giữ đúng. Điều kiện phải đúng sau khi khởi tạo và sau mỗi thao tác công khai được gọi là **bất biến của đối tượng**.

Ta thường giới hạn quyền ghi vào field hoặc setter, rồi mở các phương thức có ý nghĩa cho bên gọi. Mỗi phương thức thực thi quy tắc cập nhật tại một nơi. Nhờ vậy, nhiều nơi có thể yêu cầu cùng một hành vi và cùng tuân theo một quy tắc.

`private` hỗ trợ kiểm soát truy cập. Hiệu quả đóng gói còn phụ thuộc vào các cách cập nhật mà class cung cấp. Nếu có một setter công khai chấp nhận mọi giá trị, bên ngoài vẫn có thể đưa đối tượng vào trạng thái sai.

**Ví dụ 5 — Nhận sát thương và hồi máu trong giới hạn**

```csharp
using System;

class Tank
{
    private string name;

    public string Name => name;
    public int MaxHp { get; }
    public int Hp { get; private set; }

    public Tank(string name, int maxHp)
    {
        this.name = name;
        MaxHp = Math.Max(1, maxHp);
        Hp = MaxHp;
    }

    public void TakeDamage(int damage)
    {
        if (damage <= 0) return;

        Hp = Math.Max(0, Hp - damage);
    }

    public void Heal(int amount)
    {
        if (amount <= 0) return;

        Hp += Math.Min(amount, MaxHp - Hp);
    }
}

class Program
{
    static void Main()
    {
        Tank tank = new Tank("Armor", 80);
        tank.TakeDamage(25);
        Console.WriteLine($"{tank.Name}: {tank.Hp}/{tank.MaxHp}");

        tank.Heal(40);
        Console.WriteLine($"{tank.Name}: {tank.Hp}/{tank.MaxHp}");

        tank.TakeDamage(90);
        Console.WriteLine($"{tank.Name}: {tank.Hp}/{tank.MaxHp}");

        tank.Heal(-2);
        Console.WriteLine($"{tank.Name}: {tank.Hp}/{tank.MaxHp}");
    }
}
```

**Kết quả mong đợi**

```text
Armor: 55/80
Armor: 80/80
Armor: 0/80
Armor: 0/80
```

**Phân tích ví dụ**

Bất biến của `Tank` là `0 <= Hp <= MaxHp`. `Hp` có `private set`. Bên ngoài đọc HP và yêu cầu cập nhật qua `TakeDamage` hoặc `Heal`. Xe bắt đầu với 80 HP. Nhận 25 sát thương còn 55. Hồi 40 chỉ tăng đến 80 vì xe thiếu 25 HP. Nhận 90 đưa HP về 0, còn hồi −2 bị bỏ qua. Quy tắc này nằm trong `Tank` nên UI và mã va chạm đều có thể gọi chung.

**Kiểm tra nhanh.** Khi xe trúng đạn và nhận 20 sát thương, em nên gán trực tiếp `tank.Hp` hay gọi `tank.TakeDamage(20)`? Vì sao?

## 6. Kế thừa (Inheritance) — Khi xe con là một loại xe cha

### 6.1. Nhận ra quan hệ “là một loại”

**Lý thuyết chung**

**Kế thừa** cho phép một lớp dẫn xuất dùng lại các thành viên của lớp cơ sở theo phạm vi truy cập, đồng thời bổ sung hành vi hoặc dữ liệu riêng. Cú pháp C# là `class LopCon : LopCha`.

Quan hệ này phù hợp khi lớp con **là một loại** của lớp cha. Đối tượng lớp con có thể dùng tại nơi cần kiểu cha và tiếp tục đáp ứng những gì kiểu cha cam kết. Lớp cha giữ phần chung, lớp con phát triển phần riêng.

Với quan hệ **có một**, một kiểu giữ đối tượng kiểu khác trong field hoặc property. Hỏi hai kiểu liên hệ thế nào trước khi quyết định cách tái sử dụng code.

**Ví dụ 6.1 — Xe trinh sát dùng mô tả chung của Vehicle**

```csharp
using System;

class Vehicle
{
    private string name;

    protected Vehicle(string name)
    {
        this.name = name;
    }

    public string Describe()
    {
        return $"Xe: {name}";
    }
}

class ScoutTank : Vehicle
{
    public int Speed => 8;

    public ScoutTank(string name) : base(name) { }
}

class Program
{
    static void Main()
    {
        ScoutTank scout = new ScoutTank("Swift");
        Console.WriteLine(scout.Describe());
        Console.WriteLine($"Toc do: {scout.Speed}");
    }
}
```

**Kết quả mong đợi**

```text
Xe: Swift
Toc do: 8
```

**Phân tích ví dụ**

`ScoutTank` là một loại `Vehicle`. Nó dùng `Describe()` từ lớp cha và bổ sung `Speed`. Khi thay nội dung mô tả trong `Vehicle`, xe trinh sát dùng nội dung mới. Xe tăng có một khẩu pháo, nên có thể lưu pháo trong một thành viên riêng.

**Kiểm tra nhanh.** Đổi mẫu mô tả trong `Vehicle.Describe()`. Dòng nào thay đổi khi chạy lại? Với xe tăng và pháo, quan hệ là “một loại” hay “có một”?

### 6.2. Dùng base để khởi tạo và gọi phương thức lớp cha

**Lý thuyết chung**

Trong constructor lớp con, cú pháp `: base(...)` truyền đối số để gọi constructor lớp cha. Constructor cha khởi tạo phần trạng thái mà lớp cha quản lý.

Trong một phương thức của lớp con, `base.PhuongThuc()` gọi phần triển khai tương ứng của lớp cha. Cách này cho phép lớp con sử dụng kết quả chung rồi bổ sung hành vi riêng. Thành viên cần gọi phải truy cập được từ lớp con.

**Ví dụ 6.2 — Dùng lại constructor và nội dung Describe của lớp cha**

```csharp
using System;

class Vehicle
{
    public string Name { get; }

    protected Vehicle(string name)
    {
        Name = name;
    }

    public string Describe()
    {
        return $"Xe: {Name}";
    }
}

class ScoutTank : Vehicle
{
    public ScoutTank(string name) : base(name) { }

    public string DescribeScout()
    {
        return base.Describe() + " [Scout]";
    }
}

class Program
{
    static void Main()
    {
        ScoutTank scout = new ScoutTank("Swift");
        Console.WriteLine(scout.Describe());
        Console.WriteLine(scout.DescribeScout());
    }
}
```

**Kết quả mong đợi**

```text
Xe: Swift
Xe: Swift [Scout]
```

**Phân tích ví dụ**

`ScoutTank(string name) : base(name)` để `Vehicle` khởi tạo `Name`. Dòng `scout.Describe()` dùng phương thức được kế thừa. `DescribeScout()` gọi `base.Describe()` rồi nối hậu tố `[Scout]`, vì vậy hai dòng cùng có phần `Xe: Swift`.

**Kiểm tra nhanh.** Đổi mẫu mô tả ở `Vehicle.Describe()`. Dự đoán cả hai dòng sẽ đổi như thế nào.

## 7. Phạm vi truy cập: public, private, protected, internal

**Lý thuyết chung**

**Access modifier** quy định đoạn code nào được truy cập một kiểu hoặc thành viên. Chọn mức truy cập theo trách nhiệm của từng phần chương trình.

| Từ khóa | Phạm vi truy cập cơ bản |
| --- | --- |
| `public` | Nơi có thể truy cập kiểu chứa thành viên. |
| `private` | Bên trong kiểu khai báo thành viên. |
| `protected` | Trong lớp khai báo và lớp dẫn xuất, theo quy tắc truy cập qua đối tượng của C#. |
| `internal` | Bên trong cùng assembly. Một project Console đơn giản thường tạo một assembly. |

**Ví dụ 7 — Cùng một xe, mỗi nơi được dùng những thành viên khác nhau**

```csharp
using System;

class Vehicle
{
    private int _fuel = 40;
    protected int Speed = 8;
    internal string TeamCode = "Blue";
    public string Name { get; } = "Swift";

    protected int ReadFuel()
    {
        return _fuel;
    }
}

class Scout : Vehicle
{
    public string Report()
    {
        return $"Toc do: {Speed} | Nhien lieu: {ReadFuel()}";
    }
}

class Program
{
    static void Main()
    {
        Scout scout = new Scout();
        Console.WriteLine(scout.Name);
        Console.WriteLine(scout.Report());
        Console.WriteLine(scout.TeamCode);
    }
}
```

**Kết quả mong đợi**

```text
Swift
Toc do: 8 | Nhien lieu: 40
Blue
```

**Phân tích ví dụ**

`Main` đọc được `Name` vì property là `public` và đọc được `TeamCode` vì cùng assembly. `Scout.Report()` dùng được `Speed` và `ReadFuel()` vì chúng là `protected`. Field `_fuel` là `private` trong `Vehicle`, nên lớp cha đọc nó bên trong `ReadFuel()`.

**Kiểm tra nhanh.** Thử truy cập `scout.Speed` từ `Main`. Quan sát lỗi rồi xóa dòng thử. Giải thích vì sao `Scout.Report()` lại đọc được.

## 8. Trừu tượng (Abstraction) — Bên gọi cần biết gì?

**Lý thuyết chung**

**Trừu tượng** là chọn những đặc điểm và thao tác cần thiết để người gọi sử dụng một đối tượng. Bên gọi dựa vào **hợp đồng** của thao tác, gồm ý nghĩa của đầu vào, kết quả và hành vi mà thao tác cam kết. Chi tiết lưu dữ liệu và thuật toán nằm trong phần triển khai.

Một class cụ thể có thể đưa ra thao tác trừu tượng qua các phương thức công khai có ý nghĩa. Từ khóa `abstract` là một công cụ C# để khai báo thành viên mà lớp con sẽ triển khai. Khi phần triển khai thay đổi và hợp đồng được giữ, code gọi tiếp tục dùng được thao tác theo cùng cách.

Trừu tượng giúp xác định bên gọi cần biết gì. Đóng gói giúp xác định ai kiểm soát trạng thái và bảo vệ quy tắc cập nhật.

**Ví dụ 8 — Bên gọi chỉ dùng Fire và kết quả trả về**

```csharp
using System;

class Cannon
{
    private int ammo = 2;

    public int Fire()
    {
        if (ammo == 0) return 0;

        ammo--;
        return 20;
    }
}

class Program
{
    static void Main()
    {
        Cannon cannon = new Cannon();

        for (int shot = 1; shot <= 3; shot++)
        {
            int damage = cannon.Fire();
            Console.WriteLine($"Luot {shot}: {damage} sat thuong");
        }
    }
}
```

**Kết quả mong đợi**

```text
Luot 1: 20 sat thuong
Luot 2: 20 sat thuong
Luot 3: 0 sat thuong
```

**Phân tích ví dụ**

Hợp đồng của `Cannon.Fire()` là tiêu thụ một viên đạn và trả sát thương khi còn đạn, hoặc trả 0 khi hết đạn. `Main` gọi thao tác này ba lần và in kết quả. Nó dựa vào hợp đồng để đọc sát thương. Việc dùng field `ammo` để lưu số đạn thuộc phần triển khai, còn việc class giữ quyền cập nhật field thể hiện đóng gói.

**Kiểm tra nhanh.** Đổi số đạn ban đầu thành 3 và dự đoán dòng cuối. Nếu chuyển sang lưu từng viên đạn trong danh sách, điều gì cần giữ để `Main` tiếp tục gọi `Fire()` theo cách cũ?

## 9. Đa hình (Polymorphism) — Cùng lời gọi, hành vi theo đối tượng

Mục này bắt đầu bằng cách C# chọn bản thực thi của một phương thức, sau đó áp dụng cho một nhóm đối tượng qua cùng kiểu chung.

### 9.1. Một đối tượng với virtual, abstract và override

**Lý thuyết chung**

Trong lớp cơ sở, `virtual` khai báo một thành viên có phần triển khai mặc định mà lớp con có thể ghi đè. `abstract` khai báo một thành viên chưa có phần triển khai ở lớp cơ sở. Một lớp dẫn xuất cụ thể phải triển khai các thành viên abstract còn thiếu. Đối tượng được tạo từ một lớp cụ thể.

`override` cung cấp phần triển khai của lớp con cho thành viên ảo tương ứng. Khi gọi thành viên đó qua biến kiểu cha, C# chọn bản thực thi dựa trên kiểu thực tế của đối tượng.

**Ví dụ 9.1 — Weapon quy định Fire, Cannon cung cấp phần triển khai**

```csharp
using System;

abstract class Weapon
{
    public virtual string Describe()
    {
        return "Vu khi";
    }

    public abstract int Fire();
}

class Cannon : Weapon
{
    public override string Describe()
    {
        return "Phao";
    }

    public override int Fire()
    {
        return 20;
    }
}

class Program
{
    static void Main()
    {
        Weapon weapon = new Cannon();

        Console.WriteLine(weapon.Describe());
        Console.WriteLine($"Sat thuong: {weapon.Fire()}");
    }
}
```

**Kết quả mong đợi**

```text
Phao
Sat thuong: 20
```

**Phân tích ví dụ**

`Weapon weapon = new Cannon()` tạo biến khai báo kiểu `Weapon` giữ đối tượng `Cannon`. `weapon.Describe()` chạy bản ghi đè và trả `Phao`. `Weapon.Fire()` quy định thao tác, còn `Cannon.Fire()` cung cấp giá trị 20.

**Kiểm tra nhanh.** Nếu bỏ bản override `Describe()` trong `Cannon`, dự đoán dòng đầu rồi chạy thử. `Fire()` vẫn cần phần triển khai vì sao?

### 9.2. Duyệt nhiều đối tượng qua cùng kiểu cha

**Lý thuyết chung**

**Đa hình** cho phép nhiều đối tượng được dùng thông qua cùng một kiểu chung. Bên gọi viết một lời gọi theo thao tác mà kiểu chung cung cấp. Với phương thức ảo trong C#, **kiểu khai báo** quyết định lời gọi nào hợp lệ khi biên dịch, còn **kiểu thực tế** quyết định bản ghi đè chạy lúc thực thi.

Khi xử lý một nhóm đối tượng, mỗi phần tử có thể thuộc một lớp con khác nhau. Vòng lặp dùng cùng một thao tác. Mỗi lớp con cung cấp phần khác biệt của mình. Khi bổ sung một lớp con đáp ứng hợp đồng chung, vòng lặp có thể xử lý nó theo cùng cách.

**Ví dụ 9.2 — Cùng lời gọi Fire, hai loại xe cho sát thương khác nhau**

```csharp
using System;
using System.Collections.Generic;

abstract class TankBase
{
    public string Name { get; }

    protected TankBase(string name)
    {
        Name = name;
    }

    public virtual string Describe()
    {
        return $"Tank: {Name}";
    }

    public abstract int Fire();
}

class HeavyTank : TankBase
{
    public HeavyTank(string name) : base(name) { }

    public override string Describe()
    {
        return base.Describe() + " [Heavy]";
    }

    public override int Fire() => 40;
}

class ScoutTank : TankBase
{
    public ScoutTank(string name) : base(name) { }
    public override int Fire() => 15;
}

class Program
{
    static void Main()
    {
        List<TankBase> team = new List<TankBase>
        {
            new HeavyTank("Armor"),
            new ScoutTank("Swift")
        };
        foreach (TankBase tank in team)
        {
            Console.WriteLine($"{tank.Describe()} | Damage: {tank.Fire()}");
        }
    }
}
```

**Kết quả mong đợi**

```text
Tank: Armor [Heavy] | Damage: 40
Tank: Swift | Damage: 15
```

**Phân tích ví dụ**

`List<TankBase>` chứa hai đối tượng thuộc hai lớp con. Biến `tank` có kiểu khai báo `TankBase` ở cả hai lượt. Ở lượt đầu, `HeavyTank.Fire()` trả 40 và bản ghi đè `Describe()` thêm `[Heavy]`. Ở lượt sau, `ScoutTank.Fire()` trả 15 và xe dùng mô tả mặc định từ lớp cha.

**Kiểm tra nhanh.** Xác định kiểu khai báo của `tank`, kiểu thực tế của từng xe và bản `Fire()` chạy. Đảo vị trí hai xe và dự đoán thứ tự đầu ra. Nếu thêm xe hỗ trợ, hành vi bắn mới được đặt ở đâu?

| Lượt | Kiểu khai báo | Đối tượng thực tế | Bản thực thi |
| --- | --- | --- | --- |
| 1 | `TankBase` | `HeavyTank` | `HeavyTank.Fire()` trả 40 |
| 2 | `TankBase` | `ScoutTank` | `ScoutTank.Fire()` trả 15 |

| Tính chất | Câu hỏi để nhận ra trong code |
| --- | --- |
| Đóng gói | Ai giữ HP trong giới hạn khi xe nhận sát thương? |
| Kế thừa | Xe trinh sát dùng được ở đâu cần một phương tiện? |
| Trừu tượng | Người gọi cần biết gì để dùng `Cannon.Fire()`? |
| Đa hình | Vì sao cùng lời gọi `tank.Fire()` có thể trả 40 hoặc 15? |

## 10. static: dữ liệu và thao tác thuộc về kiểu

**Lý thuyết chung**

Thành viên **instance** thuộc từng đối tượng. Thành viên **`static`** thuộc kiểu. Một field tĩnh được dùng chung giữa các đối tượng của kiểu đó. Phương thức tĩnh thường được gọi qua tên kiểu. Nó có thể xử lý dữ liệu nhận qua tham số và truy cập thành viên tĩnh. Để dùng dữ liệu instance, nó cần tham chiếu đến một đối tượng cụ thể.

`static class` nhóm các thành viên tĩnh và được sử dụng trực tiếp qua tên kiểu. Khi quyết định đặt một giá trị ở đâu, hãy hỏi giá trị đó thuộc riêng từng đối tượng hay thuộc về cả kiểu.

**Ví dụ 10 — Đếm số vũ khí đã tạo và tính sát thương bằng static class**

```csharp
using System;

class Weapon
{
    private static int _createdCount;
    public static int CreatedCount => _createdCount;
    public int BaseDamage { get; }

    public Weapon(int baseDamage)
    {
        BaseDamage = baseDamage;
        _createdCount++;
    }
}

static class DamageMath
{
    public static int Calculate(int attack, int armor)
    {
        return Math.Max(0, attack - armor);
    }
}

class Program
{
    static void Main()
    {
        Weapon basic = new Weapon(10);
        Weapon heavy = new Weapon(25);

        Console.WriteLine($"Basic: {basic.BaseDamage}");
        Console.WriteLine($"Heavy: {heavy.BaseDamage}");
        Console.WriteLine($"So vu khi da tao: {Weapon.CreatedCount}");
        Console.WriteLine($"Sat thuong sau giap: {DamageMath.Calculate(25, 5)}");
    }
}
```

**Kết quả mong đợi**

```text
Basic: 10
Heavy: 25
So vu khi da tao: 2
Sat thuong sau giap: 20
```

**Phân tích ví dụ**

Mỗi `Weapon` có một `BaseDamage` riêng. Cả hai lần chạy constructor đều tăng cùng field tĩnh `_createdCount`, nên `Weapon.CreatedCount` là 2. `DamageMath.Calculate(25, 5)` được gọi qua tên static class và trả 20.

**Kiểm tra nhanh.** Tạo thêm một vũ khí. Bộ đếm chung tăng thế nào, sát thương của hai vũ khí cũ ra sao?

**Ví dụ bổ sung — HP của từng xe và bộ đếm chung của `Tank`**

Hai xe tăng có HP riêng. Khi tạo thêm xe, constructor tăng cùng một bộ đếm. Để thấy sự khác nhau giữa phương thức tĩnh và phương thức instance, ta tính sát thương giảm bởi giáp trước, rồi áp dụng sát thương lên một xe cụ thể.

```csharp
using System;

class Tank
{
    private static int _createdCount;

    public static int CreatedCount
    {
        get { return _createdCount; }
    }

    public int Hp { get; private set; } = 100;

    public Tank()
    {
        _createdCount++;
    }

    public void TakeDamage(int damage)
    {
        if (damage > 0)
        {
            Hp = Math.Max(0, Hp - damage);
        }
    }

    public static int CalculateArmoredDamage(int damage)
    {
        return Math.Max(0, damage / 2);
    }
}

class Program
{
    static void Main()
    {
        Tank armor = new Tank();
        Tank scout = new Tank();

        armor.TakeDamage(30);

        Console.WriteLine($"HP của Armor: {armor.Hp}");
        Console.WriteLine($"HP của Scout: {scout.Hp}");
        Console.WriteLine($"Số xe đã tạo: {Tank.CreatedCount}");

        int damage = Tank.CalculateArmoredDamage(30);
        Console.WriteLine($"Sát thương sau giáp: {damage}");

        armor.TakeDamage(damage);
        Console.WriteLine($"HP của Armor sau đòn tiếp theo: {armor.Hp}");
    }
}
```

**Kết quả mong đợi**

```text
HP của Armor: 70
HP của Scout: 100
Số xe đã tạo: 2
Sát thương sau giáp: 15
HP của Armor sau đòn tiếp theo: 55
```

**Phân tích và cách áp dụng**

- **Dữ liệu riêng của từng xe.** `Hp` là instance property. Gọi `armor.TakeDamage(30)` chỉ giảm HP của `armor`. Tên và số đạn của từng xe cũng phù hợp với thành viên instance.
- **Dữ liệu dùng chung.** Hai lần `new Tank()` tăng cùng field `_createdCount`. `private` giới hạn truy cập trực tiếp vào field trong lớp `Tank`, còn `static` làm field thuộc về lớp. Bên ngoài đọc bộ đếm qua static property `Tank.CreatedCount`. Bộ đếm ghi tổng số xe đã tạo.
- **Tính toán và áp dụng.** `Tank.CalculateArmoredDamage(30)` chỉ cần đối số để tính ra 15. `armor.TakeDamage(damage)` cần đối tượng `armor` để biết HP nào phải cập nhật. Phương thức `static` không có `this`. Nếu một phương thức tĩnh cần đọc HP, nó có thể nhận tham số `Tank tank` rồi đọc `tank.Hp`.
- **Nhóm các hàm dùng chung.** `Math.Max()` được gọi qua tên lớp `Math`. Khi nhiều hàm tính sát thương cần dùng ở nhiều nơi, có thể nhóm chúng vào một `static class` như `DamageMath` trong ví dụ trên và gọi trực tiếp qua tên lớp.

**Kiểm tra nhanh.** Thêm `Tank support = new Tank();` ngay trước dòng in số xe. Dự đoán `Tank.CreatedCount` và `support.Hp`, rồi in cả hai để kiểm tra.

## 11. const và readonly

**Lý thuyết chung**

- **`const`** dùng cho giá trị cố định mà C# biết khi biên dịch, như số `10`. Đọc hằng qua tên class.
- **`readonly`** dùng cho field có thể nhận giá trị khi tạo đối tượng. Sau đó field giữ giá trị đã gán. Mỗi đối tượng có thể nhận một giá trị khác nhau.

**Trường hợp áp dụng**

- Dùng `const` khi mọi trận trong game đều có giới hạn cố định là 4 người chơi.
- Dùng `readonly` cho mã xe truyền vào constructor. Mỗi xe có mã riêng và giữ mã đó suốt vòng đời.

**Ví dụ 11 — Mức tối đa chung, sát thương gốc khởi tạo theo từng vũ khí**

```csharp
using System;

class Weapon
{
    public const int MaxLevel = 10;
    private readonly int _baseDamage;

    public int BaseDamage => _baseDamage;

    public Weapon(int baseDamage)
    {
        _baseDamage = baseDamage;
    }
}

class Program
{
    static void Main()
    {
        Weapon basic = new Weapon(10);
        Weapon heavy = new Weapon(25);

        Console.WriteLine($"Cap toi da: {Weapon.MaxLevel}");
        Console.WriteLine($"Basic: {basic.BaseDamage}");
        Console.WriteLine($"Heavy: {heavy.BaseDamage}");
    }
}
```

**Kết quả mong đợi**

```text
Cap toi da: 10
Basic: 10
Heavy: 25
```

**Phân tích ví dụ**

`MaxLevel` là mức trần chung bằng 10. `_baseDamage` được gán khi tạo từng vũ khí, nên `basic` có sát thương gốc 10 và `heavy` có sát thương gốc 25.

**Kiểm tra nhanh.** Chỉ ra giá trị nào lấy từ tham số constructor và giá trị nào được dùng chung như hằng.

## 12. null, null conditional ?. và null coalescing ??

**Lý thuyết chung**

- **`null`** nghĩa là biến hiện chưa trỏ đến đối tượng nào. Khi bật nullable reference types, dấu `?` sau kiểu tham chiếu báo rằng biến có thể nhận `null`.
- **`?.`** đọc thành viên nếu có đối tượng. Nếu chưa có, biểu thức cho kết quả `null`.
- **`??`** dùng giá trị bên phải khi kết quả bên trái là `null`.

**Trường hợp áp dụng**

Ở màn hình chọn mục tiêu, dùng `target?.Name ?? "Chua chon muc tieu"` để hiện tên khi đã chọn hoặc hiện thông báo mặc định khi chưa chọn.

**Ví dụ 12 — Tên hiển thị mặc định khi chưa có profile**

```csharp
using System;

class PlayerProfile
{
    public string Name { get; }

    public PlayerProfile(string name)
    {
        Name = name;
    }
}

class Program
{
    static void Main()
    {
        PlayerProfile? profile = null;
        Console.WriteLine(profile?.Name ?? "Guest");

        profile = new PlayerProfile("HIT");
        Console.WriteLine(profile?.Name ?? "Guest");
    }
}
```

**Kết quả mong đợi**

```text
Guest
HIT
```

**Phân tích ví dụ**

Chưa có hồ sơ thì `profile?.Name` cho `null` và `??` chọn `"Guest"`. Khi hồ sơ đã có tên, chương trình in `"HIT"`.

**Kiểm tra nhanh.** Ở hai lần in, giá trị bên trái `??` lần lượt là gì?

## 13. Tham số ref và out

**Nhớ nhanh.** `ref` dùng giá trị đang có và có thể sửa nó. `out` nhận một giá trị do phương thức gán.

### 13.1. ref: đọc và sửa biến đã có giá trị

**Lý thuyết chung**

Với `ref`, phương thức có thể đọc và gán lại trực tiếp biến của bên gọi. Biến phải có giá trị trước khi gọi. Viết `ref` ở cả khai báo tham số và lời gọi.

**Trường hợp áp dụng**

Dùng khi một thao tác cần sửa trực tiếp các biến đã có. Ví dụ, đổi hai điểm số đang lưu trong hai biến bằng một lời gọi `Swap`.

**Ví dụ 13.1 — Đổi chỗ hai biến của bên gọi**

```csharp
using System;

class Program
{
    static void Swap(ref int first, ref int second)
    {
        int temporary = first;
        first = second;
        second = temporary;
    }

    static void Main()
    {
        int left = 10, right = 20;
        Swap(ref left, ref right);
        Console.WriteLine($"Sau Swap: {left}, {right}");
    }
}
```

**Kết quả mong đợi**

```text
Sau Swap: 20, 10
```

**Phân tích ví dụ**

`first` làm việc trực tiếp với `left`, còn `second` làm việc với `right`. Khi `Swap` đổi hai giá trị, `Main` đọc được `left = 20` và `right = 10`.

**Kiểm tra nhanh.** Bỏ `ref` ở cả khai báo và lời gọi rồi dự đoán giá trị của `left` và `right` sau `Swap`.

### 13.2. out: nhận kết quả do phương thức gán

**Lý thuyết chung**

Với `out`, phương thức gán kết quả vào một biến của bên gọi. Có thể khai báo biến ngay trong lời gọi, như `out int damage`. Phương thức phải gán giá trị trước khi trả về bình thường.

**Trường hợp áp dụng**

Khi người chơi nhập số vào ô cài đặt, `int.TryParse` dùng `bool` để báo có đọc được số hay không và dùng `out` để đưa số đã đọc về cho chương trình.

**Ví dụ 13.2 — Đọc sát thương từ chuỗi**

```csharp
using System;

class Program
{
    static void Main()
    {
        if (int.TryParse("25", out int damage))
        {
            Console.WriteLine($"Sat thuong: {damage}");
        }

        if (!int.TryParse("abc", out _))
        {
            Console.WriteLine("Sat thuong khong hop le");
        }
    }
}
```

**Kết quả mong đợi**

```text
Sat thuong: 25
Sat thuong khong hop le
```

**Phân tích ví dụ**

Chuỗi `"25"` đọc được, nên hàm trả `true` và `damage` nhận 25. Chuỗi `"abc"` cho `false`. Lời gọi thứ hai dùng `out _` để bỏ qua số và chỉ kiểm tra việc đọc có thành công hay không.

**Kiểm tra nhanh.** Với chuỗi `"0"`, hãy kiểm tra cả kết quả `bool` và số nhận được qua `out`.

## Thực hành tổng hợp

### Bài 1 — Giáp xe tăng

Tạo class `Shield` với `Points` ban đầu là 50 và đọc được từ bên ngoài. Viết `Absorb(int damage)` để trừ giáp khi sát thương dương và giữ giáp tối thiểu bằng 0. Tạo hai khiên riêng. Gọi `Absorb` lần lượt với 20, 60 và −5 trên khiên A. In kết quả sau mỗi đòn, rồi in giáp của khiên B.

**Kết quả mong đợi**

```text
A: 30
A: 0
A: 0
B: 50
```

**Gợi ý**

- Xem mục 5 rồi chọn cách khai báo để lớp `Shield` kiểm soát việc cập nhật `Points`.
- Kiểm tra sát thương trước khi trừ. Xác định cách giữ kết quả từ 0 trở lên.
- Dùng hai lần `new` để tạo hai khiên và đối chiếu trạng thái của từng đối tượng. Thử thêm sát thương 0 trên khiên B để kiểm tra giáp vẫn là 50.

**Giải thích sau khi chạy.** Nêu một câu mô tả hợp đồng của `Absorb` cho người gọi. Chỉ ra class chịu trách nhiệm giữ `Points` hợp lệ.

### Bài 2 — Thêm xe hỗ trợ

Dùng đội xe trong ví dụ 9.2. Thêm lớp `SupportTank` kế thừa `TankBase`, nhận tên qua constructor, ghi đè `Fire()` để trả 25 và ghi đè `Describe()` để thêm hậu tố ` [Support]` vào mô tả chung. Thêm xe tên `Aid` vào cuối đội. Duyệt danh sách qua kiểu `TankBase`, in từng xe và tổng sát thương.

**Kết quả mong đợi**

```text
Tank: Armor [Heavy] | Damage: 40
Tank: Swift | Damage: 15
Tank: Aid [Support] | Damage: 25
Tong sat thuong: 80
```

**Gợi ý**

- Xem cách lớp con chuyển tên cho constructor lớp cha.
- Tái sử dụng phần mô tả chung qua `base.Describe()`.
- Dùng giá trị `Fire()` của từng xe để cộng tổng. Giữ vòng lặp gọi qua kiểu `TankBase`. Thử thêm một `SupportTank` nữa. Tổng sát thương phải tăng thêm 25.

**Giải thích sau khi chạy.** Chỉ ra kiểu khai báo của biến vòng lặp và bản `Fire()` thực thi khi gặp `Aid`.
