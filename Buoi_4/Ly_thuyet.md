# Lập trình hướng đối tượng trong C#

[← Lý thuyết buổi 3](../Buoi_3/Ly_thuyet.md) · [Thực hành](#thực-hành-tổng-hợp)

Bắt đầu bằng hai xe tăng có tên và HP, sau đó thêm từng hành vi. Mỗi mục có lý thuyết, ví dụ và kết quả để tự đối chiếu. Thay toàn bộ `Program.cs` khi chạy ví dụ mới. Bật nullable reference types ở mục 12.

## 1. Class, đối tượng, new và constructor

**Lý thuyết**

Bắt đầu bằng hai xe tăng có tên và HP. `class Tank` mô tả mỗi xe lưu dữ liệu gì. **Object** là một xe cụ thể được tạo từ class. `new Tank("Armor", 100)` tạo xe và gọi **constructor** để đặt tên, HP ban đầu. Constructor trùng tên class và không khai báo kiểu trả về.

`Tank armor` khai báo biến kiểu `Tank`. `Name` và `Hp` là **field**, tức dữ liệu nằm trong từng đối tượng. Tạm dùng `public` để `Main` đọc và gán được. Ta sẽ quay lại kiểm tra cách này ở mục 5.

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

**Kiểm tra nhanh.** Khoanh hai dòng `new`. Sau đó thay dòng tạo `swift` bằng `Tank swift = armor;`. Hai biến lúc này cùng tham chiếu đến một xe. Dự đoán cả hai dòng in.

## 2. Phương thức instance: giao việc cho từng đối tượng

**Lý thuyết**

Ở mục 1, `Main` tự gán HP mới. Ta có thể đặt thao tác nhận sát thương trong class bằng phương thức `TakeDamage`. Cách gọi giống hàm đã học và có thêm đối tượng nhận lời gọi trước dấu chấm. Trong `armor.TakeDamage(20)`, `armor` là xe thực hiện thao tác. Đối số `20` được truyền vào tham số `damage`.

Phương thức instance có thể đọc và cập nhật field của đối tượng hiện tại. Ví dụ này chỉ trừ HP để quan sát xe nào đổi. Mục 5 sẽ bổ sung quy tắc về HP hợp lệ.

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

**Kiểm tra nhanh.** Nếu `armor` đang có 80 HP và nhận tiếp 100 sát thương, công thức hiện tại cho ra bao nhiêu? Giữ câu hỏi này để mở đầu phần đóng gói.

## 3. Field, property và expression body

### 3.1. Chỗ lưu dữ liệu và cách truy cập

**Lý thuyết**

Field là biến lưu dữ liệu của đối tượng. Field `private int _hp` giữ HP bên trong class. Property là thành viên cho phép đọc hoặc gán theo quy tắc đã khai báo.

| Cú pháp | Ý nghĩa |
| --- | --- |
| `public string Name { get; set; }` | Auto-property. Bên ngoài có thể đọc và gán. Trình biên dịch tạo phần lưu dữ liệu. |
| `public int Hp { get { return _hp; } }` | Getter lấy giá trị hiện tại của field `_hp`. Bên ngoài chỉ đọc. |
| `public int Score { get; private set; }` | Bên ngoài đọc được. Các phương thức trong class có thể gán. |

Khi dùng getter dựa trên field, mỗi lần đọc property lấy giá trị hiện tại của field. Ví dụ đầu dùng `get { return _hp; }`. Mục 3.2 viết lại getter bằng biểu thức.

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
    }
}
```

**Kết quả mong đợi**

```text
Armor: 100
Armor: 70
```

**Kiểm tra nhanh.** Dòng `tank.Name = "Armor";` hợp lệ nhờ thành viên nào? Thử thêm `tank.Hp = 99;`, quan sát lỗi rồi xóa dòng đó.

### 3.2. Rút gọn getter sau khi hiểu cách đọc field

**Lý thuyết**

Expression body dùng `=>` khi thân thành viên là một biểu thức. `public int HP => _hp;` là property chỉ đọc. Nó tương đương `public int HP { get => _hp; }` hoặc getter có thân `get { return _hp; }`. Mỗi lần đọc `HP`, getter lấy giá trị hiện tại của `_hp`. Vì vậy sau khi field thay đổi, lần đọc tiếp theo thấy HP mới.

Dạng này phù hợp với getter ngắn. Dùng thân có dấu ngoặc khi cần nhiều câu lệnh.

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

**Kiểm tra nhanh.** Đổi property sang getter có `return` và kiểm tra hai kết quả vẫn giống nhau.

## 4. this: tham chiếu đến đối tượng hiện tại

**Lý thuyết**

`this` là tham chiếu tới đối tượng đang chạy constructor hoặc phương thức instance. Trong `this.name = name`, `this.name` là field của xe đang khởi tạo, còn `name` là tham số. Trong `armor.Describe()`, `this` chỉ tới xe `armor`. Mục 10 sẽ đối chiếu `this` với phương thức tĩnh.

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

**Kiểm tra nhanh.** Ở lần gọi thứ hai, `this.name` là tên của xe nào?

## 5. Đóng gói (Encapsulation) — Đối tượng tự giữ HP hợp lệ

**Lý thuyết**

**Tình huống.** Mục 1 cho phép `armor.Hp = -100`. Mục 2 có `TakeDamage` nhưng vẫn có thể đưa HP xuống dưới 0. Nếu `Main`, giao diện và mã va chạm tự kiểm tra, mỗi nơi phải nhớ cùng một quy tắc.

**Bản chất.** Đối tượng giữ dữ liệu cùng những thao tác có quyền thay đổi dữ liệu. Quy tắc của `Tank` là `0 <= Hp <= MaxHp`. Bên ngoài có thể đọc `Hp` và yêu cầu `TakeDamage` hoặc `Heal`. `Tank` quyết định mức HP cuối cùng. Khi có thêm người gọi, quy tắc vẫn chỉ có một nơi thực thi.

- **Field** lưu dữ liệu. **Property** kiểm soát cách đọc hoặc gán. `Name => name` là getter viết gọn. `MaxHp { get; }` cho phép khởi tạo trong constructor. `Hp { get; private set; }` cho bên ngoài đọc và cho class cập nhật.
- `this.name = name` phân biệt field của đối tượng hiện tại với tham số constructor.
- Quyền truy cập là công cụ để bảo vệ trạng thái. Nếu field là `private` nhưng property `Hp { get; set; }` cho phép gán tự do, lời gọi `tank.Hp = -100` vẫn có thể phá quy tắc.

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

**Kiểm tra nhanh.** Đổi lượng hồi từ 40 thành 10. Dự đoán HP sau khi hồi. Nếu UI cần hồi máu, nó gọi phương thức nào để dùng chung quy tắc? Giải thích vì sao một field private vẫn có thể bị cập nhật sai nếu property cho gán tự do.

## 6. Kế thừa (Inheritance) — Khi xe con là một loại xe cha

### 6.1. Nhận ra quan hệ “là một loại”

**Lý thuyết**

**Tình huống.** Xe trinh sát cần phần mô tả tên của một phương tiện, đồng thời có tốc độ riêng. Nếu mỗi xe viết lại quy tắc đặt tên và hiển thị, thay đổi mẫu mô tả sẽ phải sửa ở nhiều lớp.

**Bản chất.** Kế thừa biểu diễn quan hệ **là một loại**. `ScoutTank : Vehicle` cho biết một xe trinh sát có thể dùng ở nơi cần `Vehicle` và giữ hành vi đã cam kết của `Vehicle`. Lớp cha giữ phần thực sự chung. Lớp con bổ sung phần riêng.

- `base(name)` gọi constructor lớp cha để khởi tạo tên. `ScoutTank` dùng `Describe()` kế thừa từ `Vehicle`.
- Field tên `private` thuộc phần cài đặt của `Vehicle`. Lớp con dùng những thao tác được lớp cha cho phép truy cập.
- Với quan hệ **có một**, dùng một thành viên chứa đối tượng. Xe tăng **có** pháo, nên có thể giữ một field kiểu `Cannon`. Kiểm tra quan hệ kiểu trước khi chọn kế thừa để chia sẻ code.

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

**Kiểm tra nhanh.** Đổi mẫu mô tả trong `Vehicle.Describe()`. Dòng nào thay đổi khi chạy lại? Với xe tăng và pháo, quan hệ là “một loại” hay “có một”?

### 6.2. Dùng base để khởi tạo và gọi phương thức lớp cha

**Lý thuyết**

Cú pháp `ScoutTank : Vehicle` chỉ lớp cha trực tiếp. Constructor `: base(name)` chuyển tham số để lớp cha khởi tạo phần dữ liệu chung. Thành viên công khai của lớp cha, như `Describe()`, dùng được qua đối tượng lớp con.

Trong thân phương thức lớp con, `base.Describe()` gọi phần triển khai ở lớp cha. Lớp con có thể dùng kết quả chung rồi bổ sung dữ liệu của mình. Kiểm tra quyền truy cập khi gọi thành viên lớp cha, nhất là với field `private` và thành viên `protected`.

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

**Kiểm tra nhanh.** Đổi mẫu mô tả ở `Vehicle.Describe()`. Dự đoán cả hai dòng sẽ đổi như thế nào.

## 7. Phạm vi truy cập: public, private, protected, internal

**Lý thuyết**

Access modifier quy định đoạn code nào có thể truy cập một thành viên. Hãy chọn phạm vi dựa trên nơi cần dùng dữ liệu hoặc thao tác.

| Từ khóa | Phạm vi trong ví dụ |
| --- | --- |
| `public` | `Main` đọc được `Name` và gọi được `Report()`. |
| `private` | `Vehicle` trực tiếp đọc được `_fuel`. `Scout` đọc qua phương thức được cung cấp. |
| `protected` | `Vehicle` và `Scout` dùng được `Speed` và `ReadFuel()`. |
| `internal` | `Main` đọc được `TeamCode` vì cùng assembly. Một project Console như demo thường tạo một assembly. |

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

**Kiểm tra nhanh.** Thử truy cập `scout.Speed` từ `Main`. Quan sát lỗi rồi xóa dòng thử. Giải thích vì sao `Scout.Report()` lại đọc được.

## 8. Trừu tượng (Abstraction) — Bên gọi cần biết gì?

**Lý thuyết**

**Tình huống.** Mã xử lý nút bắn cần ra lệnh cho pháo và biết sát thương gây ra. Nó có thể làm việc với một thao tác `Fire()` dù bên trong pháo đang lưu số đạn bằng cách nào.

**Bản chất.** Trừu tượng chọn **những gì bên gọi cần biết** và đưa ra một hợp đồng có ý nghĩa. Với ví dụ này, gọi `Fire()` tiêu thụ một viên đạn khi còn đạn và trả sát thương. Khi hết đạn, nó trả 0. Bên gọi dựa vào cam kết đó để xử lý kết quả. Lớp `Cannon` cụ thể đã thể hiện trừu tượng qua phương thức công khai. Từ khóa `abstract` ở mục 9 là một công cụ C# để khai báo hành vi mà các lớp con sẽ cung cấp.

**Cùng một thao tác dưới hai góc nhìn.** Trừu tượng trả lời bên gọi cần biết gì về `Fire()`. Đóng gói trả lời ai giữ và cập nhật số đạn để số đạn luôn hợp lệ.

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

**Kiểm tra nhanh.** Đổi số đạn ban đầu thành 3 và dự đoán dòng cuối. Nếu chuyển sang lưu từng viên đạn trong danh sách, điều gì cần giữ để `Main` tiếp tục gọi `Fire()` theo cách cũ?

## 9. Đa hình (Polymorphism) — Cùng lời gọi, hành vi theo đối tượng

Ở mục 8, bên gọi biết dùng `Fire()` nhờ hợp đồng của thao tác. Nếu có nhiều loại xe cùng bắn nhưng mỗi xe bắn khác nhau, ta cần dùng chung kiểu nào và để đâu quyết định cách bắn? Trước hết xét một biến kiểu cha giữ một đối tượng lớp con.

### 9.1. Một đối tượng với virtual, abstract và override

**Lý thuyết**

`virtual` khai báo thành viên có phần triển khai mặc định mà lớp con có thể thay thế. `abstract` khai báo thành viên chưa có phần triển khai tại lớp cha. Lớp con cụ thể phải cung cấp bản `override` cho thành viên abstract còn thiếu. Ta tạo đối tượng từ lớp cụ thể, ví dụ `new Cannon()`.

`override` giữ chữ ký tương ứng và cung cấp hành vi của lớp con. Khi gọi thành viên ảo qua biến kiểu cha, đối tượng thực tế quyết định bản thực thi. Bắt đầu theo dõi lời gọi trên một vũ khí. Sau đó áp dụng cùng quy tắc cho nhiều loại xe ở mục 9.2.

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

**Kiểm tra nhanh.** Nếu bỏ bản override `Describe()` trong `Cannon`, dự đoán dòng đầu rồi chạy thử. `Fire()` vẫn cần phần triển khai vì sao?

### 9.2. Duyệt nhiều đối tượng qua cùng kiểu cha

**Lý thuyết**

**Tình huống.** Đội có xe hạng nặng và xe trinh sát. Vòng lặp xử lý lượt cần yêu cầu từng xe bắn. Nếu vòng lặp tự chọn sát thương theo loại xe, thêm loại xe mới sẽ kéo theo việc sửa logic xử lý lượt.

**Bản chất.** Đa hình cho phép dùng nhiều đối tượng qua **cùng kiểu chung và cùng thao tác**. Kiểu khai báo `TankBase` cho trình biên dịch biết vòng lặp được gọi `Fire()`. Khi chạy, đối tượng thực tế là `HeavyTank` hay `ScoutTank` quyết định bản override thực thi. Mỗi xe giữ hành vi bắn của mình.

- `abstract Fire()` quy định mọi lớp con cụ thể phải có cách bắn. Tạo đối tượng qua lớp con cụ thể.
- `virtual Describe()` có phần thân mặc định. Lớp con có thể dùng mặc định hoặc cung cấp bản `override`.
- `base.Describe()` gọi bản của lớp cha từ trong lớp con. Lời gọi `tank.Describe()` từ vòng lặp chọn bản theo đối tượng thực tế nếu có override.

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

**Lý thuyết**

Biến tĩnh thuộc về class và được dùng chung. Biến instance thuộc từng đối tượng. Phương thức tĩnh được gọi qua tên class và có thể làm việc với dữ liệu tĩnh hoặc dữ liệu nhận qua tham số. Để truy cập dữ liệu instance, phương thức tĩnh cần có một đối tượng cụ thể.

`static class` nhóm các thành viên tĩnh. Gọi `DamageMath.Calculate(...)` trực tiếp qua tên class. Với trạng thái như HP, mỗi xe thường có một giá trị riêng.

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

**Kiểm tra nhanh.** Tạo thêm một vũ khí. Bộ đếm chung tăng thế nào, sát thương của hai vũ khí cũ ra sao?

## 11. const và readonly

**Lý thuyết**

`const` khai báo hằng xác định tại thời điểm biên dịch. Đọc hằng của class qua tên kiểu, ví dụ `Weapon.MaxLevel`. Dùng hằng khi một giá trị thật sự cố định và có thể viết dưới dạng biểu thức hằng.

Field instance `readonly` nhận giá trị khi khai báo hoặc trong constructor. Hai đối tượng có thể nhận hai giá trị khác nhau. Sau khi khởi tạo, field giữ giá trị được gán. Trong ví dụ, sát thương gốc đến từ đối số constructor nên dùng `readonly`.

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

**Kiểm tra nhanh.** Chỉ ra giá trị nào lấy từ tham số constructor và giá trị nào được dùng chung như hằng.

## 12. null, null conditional ?. và null coalescing ??

**Lý thuyết**

`null` biểu diễn trường hợp biến chưa tham chiếu đến một đối tượng. Với nullable reference types, `PlayerProfile?` khai báo khả năng này. `profile?.Name` lấy tên nếu có profile và cho kết quả null nếu chưa có. `??` chọn giá trị dự phòng khi vế trái là null.

Trong `profile?.Name ?? "Guest"`, hai toán tử phối hợp để hiển thị tên hiện có hoặc tên mặc định.

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

**Kiểm tra nhanh.** Ở hai lần in, giá trị bên trái `??` lần lượt là gì?

## 13. Tham số ref và out

Ở mục 2, ta truyền một giá trị sát thương cho phương thức. Bây giờ quay lại tham số để xem cách phương thức làm việc trực tiếp với biến của bên gọi.

### 13.1. ref: đọc và sửa biến đã có giá trị

Khi truyền theo giá trị, phương thức nhận bản sao giá trị. Nếu giá trị là tham chiếu, phương thức có thể sửa đối tượng qua tham chiếu đó. `ref` cho phép đọc và gán lại chính biến của bên gọi. Biến phải được khởi tạo trước. Viết `ref` ở khai báo tham số và ở lời gọi.

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

**Kiểm tra nhanh.** Bỏ `ref` ở cả khai báo và lời gọi rồi dự đoán giá trị của `left` và `right` sau `Swap`.

### 13.2. out: nhận kết quả do phương thức gán

`out` dùng để đưa một kết quả từ phương thức về biến của bên gọi. Biến không cần khởi tạo trước. Phương thức phải gán giá trị cho tham số `out` trước khi trả về bình thường. Trong `TryParse`, `bool` báo chuyển đổi thành công, còn biến `out` giữ số đã đọc. Đây cũng là mẫu gọi quen thuộc của `Dictionary.TryGetValue` ở buổi 3.

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
