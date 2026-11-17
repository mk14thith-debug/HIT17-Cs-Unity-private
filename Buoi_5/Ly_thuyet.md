# Buổi 5 — Interface, generic, event và thiết kế dễ mở rộng

[← Lý thuyết buổi 4](../Buoi_4/Ly_thuyet.md) · [Thực hành cuối buổi](#thực-hành-cuối-buổi)

Buổi này tiếp nối đa hình ở buổi 4. Ta sẽ dùng cùng một thao tác trên nhiều loại đối tượng, viết hàm dùng lại được với nhiều kiểu và phát thông báo khi trạng thái thay đổi.

Ưu tiên tự viết interface, thử một hàm generic và chạy ví dụ event. Đọc casting, SOLID và LINQ qua các tình huống bên dưới. Mỗi ví dụ Console là một chương trình độc lập. Thay toàn bộ `Program.cs` bằng ví dụ đang học. Bật nullable reference types cho các ví dụ dùng kiểu như `string?`.

## 1. Interface và hợp đồng

**Lý thuyết chung**

Interface mô tả một **khả năng** qua các thành viên mà kiểu triển khai cam kết cung cấp. Một tham số kiểu interface cho phép bên gọi dùng những thành viên đã khai báo trong hợp đồng. Với cách triển khai ngầm định trong bài, phương thức trong class phải có chữ ký tương ứng và là `public`.

Dùng interface khi nhiều kiểu cần đáp ứng cùng một hợp đồng. Dùng abstract class khi các lớp con còn cần chia sẻ trạng thái, constructor hoặc phần triển khai. Một class có thể kế thừa một class và triển khai nhiều interface.

**Mục đích sử dụng**

Viết logic dùng chung cho các đối tượng có cùng khả năng. Khi thêm một loại mục tiêu mới triển khai `IGetHit`, hàm xử lý trúng đạn có thể gọi cùng thao tác trên mục tiêu đó.

**Cú pháp**

```csharp
interface ITen
{
    void ThucHien(int giaTri);
}
```

Class triển khai dùng dạng `class TenLop : ITen` và cung cấp `public void ThucHien(int giaTri)`.

**Ví dụ tình huống**

Đạn gây 20 sát thương cho xe tăng và thùng gỗ. Hai đối tượng cùng đáp ứng thao tác `GetHit`, rồi tự xử lý đòn đánh.

**Khi nào dùng.** Đạn cần gây sát thương cho xe tăng, thùng gỗ hoặc những mục tiêu khác cùng có khả năng nhận đòn.

**Code mẫu — Gọi qua interface**

```csharp
using System;

interface IGetHit
{
    void GetHit(int damage);
}

class Tank : IGetHit
{
    public int Hp { get; private set; } = 100;

    public void GetHit(int damage)
    {
        if (damage <= 0) return;
        Hp = Math.Max(0, Hp - damage);
        Console.WriteLine($"Tank HP: {Hp}");
    }
}

class Crate : IGetHit
{
    public void GetHit(int damage)
    {
        if (damage > 0) Console.WriteLine("Crate bi trung dan");
    }
}

class Program
{
    static void HitTarget(IGetHit target, int damage)
    {
        target.GetHit(damage);
    }

    static void Main()
    {
        HitTarget(new Tank(), 20);
        HitTarget(new Crate(), 20);
    }
}
```

**Kết quả**

```text
Tank HP: 80
Crate bi trung dan
```

**Phân tích ví dụ**

`HitTarget` gọi `GetHit` qua `IGetHit`. Xe trừ 20 HP và in 80. Thùng gỗ dùng cách xử lý của mình để in thông báo trúng đạn.

**Kiểm tra nhanh.** Nếu thêm một mục tiêu mới thực thi `IGetHit`, hàm `HitTarget` cần biết thêm điều gì?

## 2. Overload và override

**Lý thuyết chung**

**Overload** cho phép nhiều phương thức cùng tên với danh sách tham số khác nhau. Trình biên dịch chọn phương thức phù hợp với đối số. Hai phương thức chỉ khác kiểu trả về không tạo được overload. **Override** thay phần triển khai của một thành viên ảo được kế thừa. Khi chạy lời gọi ảo, kiểu thực tế của đối tượng quyết định bản thực thi.

**Mục đích sử dụng**

- **Overload** giúp người gọi dùng cùng tên cho các biến thể của một thao tác, chẳng hạn bắn một lần hoặc truyền số lần bắn.
- **Override** giúp mỗi lớp con quyết định hành vi của mình. Đoạn code xử lý đối tượng qua kiểu cha vẫn gọi cùng một phương thức.

**Cú pháp**

| Khái niệm | Dạng khai báo |
| --- | --- |
| Overload | Cùng class khai báo `void Run()` và `void Run(int count)`, mỗi phương thức có thân riêng. |
| Override | Lớp cha khai báo `public virtual void Run()`. Lớp con cung cấp `public override void Run()`. |

**Ví dụ tình huống**

Vũ khí có hai cách gọi `Fire()` và `Fire(3)`. Laser ghi đè cách bắn của lớp cha để trả về mô tả riêng.

**Khi nào dùng.** Overload cho nhiều cách gọi một thao tác. Override cho lớp con cung cấp hành vi riêng qua hợp đồng của lớp cha.

**Code mẫu — Hai overload và một override**

```csharp
using System;

class Weapon
{
    public virtual string Fire() => "Dan thuong";

    public string Fire(int count) => $"{count} lan: {Fire()}";
}

class Laser : Weapon
{
    public override string Fire() => "Laser";
}

class Program
{
    static void Main()
    {
        Weapon weapon = new Laser();
        Console.WriteLine(weapon.Fire());
        Console.WriteLine(weapon.Fire(3));
    }
}
```

**Kết quả**

```text
Laser
3 lan: Laser
```

**Phân tích ví dụ**

Lời gọi `Fire(3)` chọn overload có một tham số. Trong thân phương thức đó, lời gọi `Fire()` chạy bản override của `Laser`, nên dòng thứ hai là `3 lan: Laser`.

**Kiểm tra nhanh.** Lời gọi nào chọn theo số đối số, và lời gọi nào chọn bản ghi đè theo đối tượng?

## 3. Generic và ràng buộc where

**Lý thuyết chung**

Generic cho phép viết một thuật toán với **tham số kiểu** như `T`. Bên gọi có thể ghi kiểu hoặc để C# suy ra từ đối số. Các biến và tham số vẫn được kiểm tra kiểu khi biên dịch, tương tự `List<int>` ở buổi 3.

Constraint `where T : IGetHit` giới hạn kiểu dùng được và cho phép thân phương thức gọi các thành viên của `IGetHit`. `where T : class` giới hạn `T` là kiểu tham chiếu. Nếu thuật toán chỉ di chuyển dữ liệu giữa hai biến cùng kiểu, như `Swap<T>`, không cần thêm constraint.

**Mục đích sử dụng**

- **Generic** giúp viết thuật toán một lần và dùng lại với nhiều kiểu. C# vẫn kiểm tra kiểu của đối số khi biên dịch.
- **Ràng buộc `where`** cho hàm biết những thao tác nào có thể gọi trên `T`. Nó cũng giúp phát hiện kiểu không phù hợp ngay tại lời gọi.

**Cú pháp**

```text
KieuTraVe TenHam<T>(T thamSo) { ... }
KieuTraVe TenHam<T>(T thamSo) where T : IKhaNang { ... }
```

Gọi bằng `TenHam<KieuCuThe>(giaTri)` hoặc `TenHam(giaTri)` khi C# suy ra được `T`. Nếu tham số là `ref T`, lời gọi cũng cần `ref`.

**Ví dụ tình huống**

Dùng cùng một hàm để đổi chỗ hai số rồi đổi chỗ hai chuỗi. Một hàm khác nhận mục tiêu có khả năng `IGetHit` để gọi thao tác nhận đòn.

**Khi nào dùng.** Một thuật toán đổi chỗ dùng cho nhiều kiểu dữ liệu. Khi thuật toán cần gọi một hành vi, dùng constraint để diễn đạt khả năng mà kiểu phải có.

**Code mẫu — Suy ra T và dùng ràng buộc**

```csharp
using System;

interface IGetHit
{
    void GetHit(int damage);
}

class PracticeTarget : IGetHit
{
    public void GetHit(int damage)
    {
        Console.WriteLine($"Muc tieu nhan: {damage}");
    }
}

class Program
{
    static void Swap<T>(ref T a, ref T b)
    {
        T temporary = a;
        a = b;
        b = temporary;
    }

    static void ApplyHit<T>(T target, int damage) where T : IGetHit
    {
        target.GetHit(damage);
    }

    static void Main()
    {
        int left = 10, right = 20;
        Swap<int>(ref left, ref right);
        Console.WriteLine($"{left}, {right}");

        string first = "Tank", second = "Crate";
        Swap(ref first, ref second);
        Console.WriteLine($"{first}, {second}");

        ApplyHit(new PracticeTarget(), 12);
    }
}
```

**Kết quả**

```text
20, 10
Crate, Tank
Muc tieu nhan: 12
```

**Phân tích ví dụ**

Hai lời gọi `Swap` dùng cùng thuật toán với `T` là `int` và `string`. Ở `ApplyHit`, constraint cho phép gọi `GetHit` và chuyển giá trị 12 cho mục tiêu.

**Kiểm tra nhanh.** `T` là kiểu gì ở từng lời gọi `Swap`? `where` cho phép `ApplyHit` gọi thành viên nào?

### Đọc trước về constraint trong Unity

**Lý thuyết**

`Component` là kiểu cơ sở của các thành phần gắn với `GameObject`. `MonoBehaviour` là một kiểu dẫn xuất của `Component`.

**Mục đích sử dụng**

Dùng cùng một hàm generic cho nhiều loại thành phần Unity. Ràng buộc cho C# biết hàm có thể dùng những thành viên nào trên `T`, chẳng hạn `enabled` khi `T` kế thừa `MonoBehaviour`.

**Cú pháp**

- `where T : Component` cho phép các kiểu thành phần như `Transform`.
- `where T : MonoBehaviour` cho phép các kiểu script kế thừa `MonoBehaviour`.

**Ví dụ tình huống**

Đọc `Transform` từ đối tượng hoặc bật một script đang gắn trên đối tượng.

**Khi nào dùng.** Chọn `Component` khi hàm cần làm việc với các thành phần nói chung. Chọn `MonoBehaviour` khi hàm cần thuộc tính của script như `enabled`.

**Code mẫu trong script Unity**

```csharp
using UnityEngine;

public static class ComponentReader
{
    public static T ReadComponent<T>(GameObject owner) where T : Component
    {
        return owner.GetComponent<T>();
    }

    public static void EnableBehaviour<T>(T behaviour) where T : MonoBehaviour
    {
        behaviour.enabled = true;
    }
}
```

**Kết quả khi gọi.** `ReadComponent<Transform>(owner)` lấy `Transform` của `owner`. Với các kiểu thành phần có thể vắng mặt, kiểm tra giá trị trả về trước khi sử dụng. `EnableBehaviour(script)` đặt `enabled` của script được truyền vào thành `true`.

## 4. Delegate, Action, Func và lambda

**Lý thuyết chung**

Delegate là một kiểu dùng để lưu và gọi phương thức có chữ ký phù hợp. Ta có thể gán một phương thức có tên hoặc một lambda vào biến delegate. Lambda mô tả tham số và biểu thức hoặc phần thân thực thi qua dấu `=>`.

| Kiểu | Chữ ký trong bài |
| --- | --- |
| `DamageRule` | Nhận hai `int`, trả `int` |
| `Action<int>` | Nhận một `int`, trả `void` |
| `Func<int, int, int>` | Nhận hai `int`, trả `int`. Kiểu cuối là kiểu trả về. |

**Mục đích sử dụng**

- **Delegate** giúp truyền hoặc thay đổi một thao tác trong lúc chương trình chạy. Ví dụ, phần xử lý đòn đánh nhận quy tắc tính sát thương từ nơi gọi.
- **`Action` và `Func`** cung cấp sẵn các kiểu delegate thường gặp. Chọn `Action` cho thao tác trả `void` và `Func` cho thao tác trả kết quả.
- **Lambda** giúp viết nhanh một thao tác ngắn ngay tại chỗ cần dùng, chẳng hạn phép tính hoặc điều kiện lọc.

**Cú pháp**

| Khái niệm | Cú pháp thường dùng |
| --- | --- |
| Delegate tự khai báo | `delegate int DamageRule(int attack, int armor);` |
| Gán phương thức có tên | `DamageRule rule = DamageAfterArmor;` |
| `Action` | `Action<int> show = hp => Console.WriteLine(hp);` |
| `Func` | `Func<int, int, int> add = (a, b) => a + b;` |
| Lambda | `x => x * 2` hoặc `(a, b) => { return a + b; }` |

**Ví dụ tình huống**

Viết phép tính sát thương sau giáp theo hai cách, một phương thức có tên và một lambda. Gán lần lượt vào biến `rule` rồi gọi với cùng đối số.

**Khi nào dùng.** Lưu một quy tắc tính sát thương để gọi sau, hoặc nhận một hành động cần thực hiện khi HP thay đổi.

**Code mẫu — Gán phương thức và lambda**

```csharp
using System;

delegate int DamageRule(int attack, int armor);

class Program
{
    static int DamageAfterArmor(int attack, int armor)
    {
        return Math.Max(0, attack - armor);
    }

    static void Main()
    {
        DamageRule rule = DamageAfterArmor;
        Console.WriteLine($"Phuong thuc: {rule(30, 10)}");

        rule = (attack, armor) => Math.Max(0, attack - armor);
        Console.WriteLine($"Lambda: {rule(30, 10)}");

        Action<int> showHp = hp => Console.WriteLine($"HP: {hp}");
        Func<int, int, int> add = (a, b) => a + b;
        showHp(80);
        Console.WriteLine($"Tong: {add(2, 3)}");
    }
}
```

**Kết quả**

```text
Phuong thuc: 20
Lambda: 20
HP: 80
Tong: 5
```

**Phân tích ví dụ**

Phương thức có tên và lambda đều nhận 30, 10 và trả 20. `showHp` thực hiện thao tác in. `add` trả tổng để `Main` in.

**Kiểm tra nhanh.** Đọc thành lời chữ ký của `Action<int>` và `Func<int, int, int>`.

## 5. Event

**Lý thuyết chung**

Event cho phép một đối tượng **phát thông báo** khi có việc xảy ra. Bên phát cập nhật trạng thái rồi gọi các phương thức xử lý đã đăng ký. Bên nghe dùng `+=` để đăng ký và `-=` để hủy. Trong demo, `?.Invoke(Hp)` gọi các phương thức xử lý khi có người nghe.

Bên phát giữ quyền phát event. Mỗi bên nghe có thể tự quyết định cách phản ứng. Nếu dùng lambda cần hủy sau này, lưu delegate vào biến và dùng cùng biến để đăng ký và hủy.

**Mục đích sử dụng**

Cho nhiều phần của game phản ứng với cùng một thay đổi. Class quản lý HP phát thông báo về HP mới. Phần giao diện có thể đổi thanh máu, còn phần âm thanh có thể phát tiếng trúng đạn. Mỗi phần tự đăng ký hành động của mình.

**Cú pháp**

| Thao tác | Cú pháp |
| --- | --- |
| Khai báo trong lớp phát | `public event Action<int>? ValueChanged;` |
| Phát trong lớp khai báo | `ValueChanged?.Invoke(newValue);` |
| Đăng ký | `source.ValueChanged += HandleValue;` |
| Hủy đăng ký | `source.ValueChanged -= HandleValue;` |

Với `Action<int>`, phương thức `HandleValue` nhận một `int` và trả `void`.

**Ví dụ tình huống**

Xe bắt đầu với 100 HP. Sau đòn 25 sát thương, bảng hiển thị nhận HP mới. Sau khi bảng hủy đăng ký, đòn tiếp theo vẫn giảm HP của xe.

**Khi nào dùng.** Một thay đổi HP có thể khiến thanh máu cập nhật và âm thanh phát. Class quản lý HP chỉ cần phát thông báo về HP mới.

Trong ví dụ này, các phương thức xử lý chạy ngay trong lời gọi phát event.

**Code mẫu — Đăng ký và hủy thông báo HP**

```csharp
using System;

class Health
{
    public int Hp { get; private set; } = 100;
    public event Action<int>? HpChanged;

    public void TakeDamage(int damage)
    {
        if (damage <= 0 || Hp == 0) return;
        Hp = Math.Max(0, Hp - damage);
        HpChanged?.Invoke(Hp);
    }
}

class Program
{
    static void ShowHp(int hp)
    {
        Console.WriteLine($"HP: {hp}");
    }

    static void Main()
    {
        Health health = new Health();
        health.HpChanged += ShowHp;
        health.TakeDamage(25);
        health.TakeDamage(0);

        health.HpChanged -= ShowHp;
        health.TakeDamage(25);
        Console.WriteLine($"HP cuoi: {health.Hp}");
    }
}
```

**Kết quả**

```text
HP: 75
HP cuoi: 50
```

**Phân tích ví dụ**

Đòn 25 đầu tiên đưa HP về 75 và gọi `ShowHp`. Đòn 0 giữ HP nguyên. Sau khi hủy đăng ký, đòn 25 tiếp theo vẫn đưa HP về 50. Dòng `HP cuoi: 50` do `Main` in.

**Kiểm tra nhanh.** Sau khi hủy `ShowHp`, HP thay đổi thế nào ở đòn tiếp theo? Phương thức nào in dòng cuối?

## 6. Casting với ép kiểu trực tiếp, as và is

**Lý thuyết chung**

Biến kiểu chung có thể giữ một đối tượng kiểu cụ thể. Với phép ép kiểu tham chiếu trong bài, `(Kieu)value` dùng khi cần chuyển sang kiểu đích và có thể ném ngoại lệ nếu kiểu thực tế không phù hợp. `as` trả `null` khi phép chuyển kiểu tham chiếu không phù hợp. `is Kieu bien` kiểm tra kiểu và cho dùng biến đã được kiểm tra trong nhánh phù hợp.

**Mục đích sử dụng**

Truy cập thành viên của một kiểu cụ thể khi biến đang được khai báo bằng kiểu chung như `object` hoặc kiểu cha. Nếu kiểu thực tế chưa chắc chắn, dùng `is` hoặc `as` để xử lý trường hợp phù hợp trước khi truy cập thành viên.

**Cú pháp**

| Cách dùng | Cú pháp |
| --- | --- |
| Ép kiểu trực tiếp | `string text = (string)value;` |
| Ép có thể nhận `null` | `string? text = value as string;` |
| Kiểm tra kiểu và dùng biến trong nhánh | `if (value is string text) { ... }` |

Đọc từng dòng như một cách dùng riêng. `as` áp dụng cho kiểu tham chiếu hoặc kiểu giá trị nullable. Dùng `is int number` để kiểm tra giá trị kiểu `int`.

**Ví dụ tình huống**

Một biến `object` ban đầu giữ chuỗi `"Tank"`, sau đó giữ số `20`. Quan sát kết quả khi kiểm tra hoặc ép kiểu ở hai thời điểm.

**Khi nào dùng.** Dùng `is IGetHit target` để kiểm tra một giá trị có hỗ trợ khả năng nhận sát thương trước khi gọi `GetHit`.

**Code mẫu — Ép kiểu trực tiếp, as và is**

```csharp
using System;

class Program
{
    static void Main()
    {
        object value = "Tank";
        string text = (string)value;
        Console.WriteLine($"Ep kieu: {text}");
        Console.WriteLine($"La string: {value is string}");

        value = 20;
        string? maybeText = value as string;
        Console.WriteLine($"as string: {maybeText ?? "(null)"}");

        if (value is int damage)
        {
            Console.WriteLine($"Sat thuong: {damage}");
        }
    }
}
```

**Kết quả**

```text
Ep kieu: Tank
La string: True
as string: (null)
Sat thuong: 20
```

**Phân tích ví dụ**

Khi `value` giữ chuỗi, phép ép sang `string` và phép kiểm tra `is string` thành công. Sau khi gán số 20, `as string` cho `null` còn `is int damage` lấy được 20.

**Kiểm tra nhanh.** Nếu `value` giữ số 20, vì sao `as string` và `is int damage` cho kết quả khác nhau?

## 7. Năm nguyên tắc SOLID

**Lý thuyết chung**

SOLID là năm nguyên tắc thiết kế giúp tổ chức trách nhiệm và điểm mở rộng. Ví dụ tập trung vào việc xe dùng vũ khí thông qua hợp đồng `IWeapon`.

| Nguyên tắc | Cách hiểu và ví dụ |
| --- | --- |
| S — Single Responsibility | Một lớp có một nhóm trách nhiệm gắn kết, một lý do chính để thay đổi. `Health` quản lý HP; lớp hiển thị nhận event để cập nhật UI. |
| O — Open/Closed | Thêm biến thể qua điểm mở rộng thay vì sửa nhiều điều kiện cũ. Thêm vũ khí bằng cách triển khai `IWeapon` và đưa vào nơi dùng. |
| L — Liskov Substitution | Lớp con dùng thay lớp cha mà giữ các cam kết về hành vi. Nếu `TakeDamage(0)` được cam kết không đổi HP thì lớp con không được biến nó thành mất máu. |
| I — Interface Segregation | Hợp đồng nhỏ theo nhu cầu. Tách `IMovable`, `IGetHit`; thùng gỗ nhận sát thương không cần có hàm di chuyển giả. |
| D — Dependency Inversion | Logic cấp cao phụ thuộc hợp đồng. `Tank` nhận `IWeapon`, thay vì tự tạo cứng một `BasicGun` bên trong. |

**Mục đích sử dụng**

Giúp thay đổi một phần của chương trình mà hạn chế phải sửa các phần liên quan. Trong ví dụ, mỗi vũ khí tự cung cấp cách bắn. Khi thêm vũ khí mới đáp ứng `IWeapon`, phần điều khiển tấn công của xe vẫn gọi qua `Fire()`.

**Cú pháp**

Mẫu dưới đây minh họa cách nhận phụ thuộc qua interface trong code SOLID.

```text
interface IHopDong { ... }
class KieuCuThe : IHopDong { ... }
public TenLop(IHopDong dependency) { ... }
```

**Ví dụ tình huống**

Một xe có thể dùng pháo thường hoặc laser. Truyền loại vũ khí muốn dùng qua constructor. Xe gọi `Fire()` qua `IWeapon`.

**Khi nào dùng.** Xe cần dùng nhiều loại vũ khí. Để thay cách bắn, truyền một đối tượng thực thi `IWeapon` vào constructor.

**Code mẫu — Đổi vũ khí qua constructor**

```csharp
using System;

interface IWeapon
{
    int Fire();
}

class BasicGun : IWeapon
{
    public int Fire() => 10;
}

class Laser : IWeapon
{
    public int Fire() => 25;
}

class ArmedTank
{
    private readonly IWeapon weapon;

    public ArmedTank(IWeapon weapon)
    {
        this.weapon = weapon;
    }

    public int Attack() => weapon.Fire();
}

class Program
{
    static void Main()
    {
        ArmedTank tankWithGun = new ArmedTank(new BasicGun());
        ArmedTank tankWithLaser = new ArmedTank(new Laser());
        Console.WriteLine($"BasicGun: {tankWithGun.Attack()}");
        Console.WriteLine($"Laser: {tankWithLaser.Attack()}");
    }
}
```

**Kết quả**

```text
BasicGun: 10
Laser: 25
```

**Phân tích ví dụ**

Hai lời gọi `Attack()` cùng đi qua `IWeapon.Fire()`. Vũ khí được truyền vào quyết định kết quả 10 hoặc 25.

**Kiểm tra nhanh.** Nếu thay `BasicGun` bằng `Laser` khi tạo xe, thao tác `Attack()` vẫn gọi qua hợp đồng nào?

## 8. Lọc và biến đổi dữ liệu bằng LINQ

**Lý thuyết chung**

LINQ cung cấp các thao tác truy vấn tập dữ liệu. Dùng `Where` để lọc, `Select` để biến đổi từng phần tử, `FirstOrDefault` để lấy kết quả đầu tiên hoặc giá trị mặc định, và `Any` để hỏi có phần tử thỏa điều kiện hay không. Trong ví dụ này, `Where` và `Select` xử lý dữ liệu khi vòng lặp duyệt kết quả.

**Mục đích sử dụng**

Diễn đạt rõ cần lấy dữ liệu nào và muốn biến đổi dữ liệu thành dạng gì. Chẳng hạn, có thể đọc truy vấn từ trái sang phải như hai bước: lọc điểm đủ cao, rồi chuyển mỗi điểm thành nhãn.

**Cú pháp**

Thêm `using System.Linq;` để dùng các thao tác sau.

| Thao tác | Cú pháp |
| --- | --- |
| Lọc | `source.Where(item => dieuKien)` |
| Biến đổi | `source.Select(item => giaTriMoi)` |
| Lấy phần tử đầu tiên thỏa điều kiện | `source.FirstOrDefault(item => dieuKien)` |
| Kiểm tra có kết quả | `source.Any(item => dieuKien)` |

**Ví dụ tình huống**

Từ danh sách điểm `40, 90, 120, 0`, lọc điểm từ 90 trở lên và tạo nhãn hiển thị. Thử tìm điểm trên 999 để xem kết quả khi không có phần tử phù hợp.

**Khi nào dùng.** Lọc các điểm đạt ngưỡng rồi tạo nhãn để hiển thị. Nếu `0` là điểm hợp lệ, dùng `Any` để biết điều kiện có kết quả.

`List<T>.Find(...)` cũng tìm phần tử đầu tiên thỏa điều kiện, nhưng thuộc kiểu `List<T>`. Với LINQ, có thể kết hợp các bước lọc và biến đổi thành một truy vấn.

**Code mẫu — Lọc điểm và tạo nhãn**

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        List<int> scores = new List<int> { 40, 90, 120, 0 };
        var highScores = scores.Where(score => score >= 90);
        var labels = highScores.Select(score => $"Score: {score}");

        foreach (string label in labels)
        {
            Console.WriteLine(label);
        }

        Console.WriteLine($"Diem cao dau tien: {scores.FirstOrDefault(score => score >= 90)}");
        Console.WriteLine($"Diem tren 999: {scores.FirstOrDefault(score => score > 999)}");
        Console.WriteLine($"Co diem tren 999: {scores.Any(score => score > 999)}");
    }
}
```

**Kết quả**

```text
Score: 90
Score: 120
Diem cao dau tien: 90
Diem tren 999: 0
Co diem tren 999: False
```

**Phân tích ví dụ**

`Where` giữ 90 và 120. `Select` tạo hai nhãn. Với điều kiện trên 999, `FirstOrDefault` trả 0 và `Any` trả `False`, nên biết không có điểm phù hợp.

**Kiểm tra nhanh.** Nếu đổi điều kiện lọc thành `score > 90`, còn nhãn nào?

## Thực hành cuối buổi

Chọn một bài. Dùng kết quả mẫu để tự kiểm tra.

### Bài 1 — In nhãn mục tiêu

Tạo hai kiểu `TankMarker` và `CrateMarker`, cùng thực thi `ILabel` với phương thức `string GetLabel()`. Nhãn của hai đối tượng lần lượt là `Tank Alpha` và `Crate B2`. Viết `PrintLabel<T>(T target) where T : ILabel` để in nhãn. Gọi cùng phương thức với từng đối tượng.

**Kết quả mong đợi**

```text
Tank Alpha
Crate B2
```

**Gợi ý**

- Khai báo interface ngoài `Main`. Kiểm tra chữ ký và phạm vi truy cập của `GetLabel` khi triển khai.
- Đặt hàm generic trong `Program`, ngoài `Main`. Đọc phần `where` để xác định hàm có thể gọi thành viên nào của `target`.
- Thử đổi nhãn của thùng thành `Crate C3`. Chỉ dòng thứ hai thay đổi.

### Bài 2 — Băng đạn phát thông báo

Tạo `AmmoMagazine` với `Ammo` ban đầu là 3 và event `Action<int>? AmmoChanged`. Mỗi lần gọi `Shoot()`, giảm một viên nếu còn đạn. Phát event một lần cho mỗi lần số đạn giảm và truyền số đạn mới.

Trong `Main`, đăng ký handler in số đạn, bắn hai lần rồi hủy handler. Bắn thêm hai lần và in số đạn cuối từ `Main`.

**Kết quả mong đợi**

```text
Dan: 2
Dan: 1
Dan cuoi: 0
```

**Gợi ý**

- Kiểm tra số đạn trước khi trừ. Trong kịch bản mẫu, lần bắn thứ tư gặp băng rỗng.
- Xác định vị trí phát event dựa trên thứ tự cập nhật dữ liệu trong demo `Health`.
- Đăng ký và hủy bằng cùng handler. Nếu dùng lambda, giữ delegate trong một biến.
- Để kiểm tra nhánh băng rỗng, giữ handler đến hết bốn lần bắn. Handler phải in thêm `Dan: 0` ở lần thứ ba và chạy tổng cộng ba lần.
