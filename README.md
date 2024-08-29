# SSC-Internship-Dev-CSharpAdvanced-Delegate
# Hướng Dẫn Về Generics trong C#
## 1. Delegate là gì?
Delegate trong C# là một kiểu dữ liệu đặc biệt, đại diện cho tham chiếu tới các phương thức. Bạn có thể hình dung delegate giống như một con trỏ trỏ tới hàm, cho phép bạn truyền phương thức như một tham số, lưu trữ chúng trong biến và gọi chúng một cách động. Delegate giúp mã nguồn trở nên linh hoạt hơn, đặc biệt là khi kết hợp với các mẫu thiết kế lập trình khác.

Ví dụ: Sử dụng Delegate cơ bản
```csharp
using System;

delegate int MathOperation(int num1, int num2);

class Program {
    static void Main(string[] args) {
        // Khởi tạo delegate với phương thức Add
        MathOperation operation = new MathOperation(Add);

        // Gọi phương thức thông qua delegate
        int result = operation(5, 7);

        // In ra kết quả
        Console.WriteLine("Sum: " + result); // Output: Sum: 12
    }

    // Phương thức phù hợp với chữ ký của delegate
    static int Add(int num1, int num2) {
        return num1 + num2;
    }
}
```
Trong ví dụ này, thay vì gọi trực tiếp phương thức `Add`, chúng ta sử dụng một delegate `operation` để gọi nó. Điều này giúp mã nguồn linh hoạt và động hơn, đặc biệt là khi kết hợp với các mẫu thiết kế lập trình khác.

## 2. Sử dụng Delegate để gọi các phương thức khác nhau
Delegate có thể được sử dụng để tham chiếu đến các phương thức khác nhau có cùng chữ ký, giúp dễ dàng thay đổi phương thức được gọi tại runtime.

Ví dụ: Chuyển đổi giữa các phương thức
```csharp
using System;

// Khai báo delegate
delegate double Operation(double a, double b);

class Program {
    static double Multiply(double a, double b) {
        return a * b;
    }

    static double Divide(double a, double b) {
        return a / b;
    }

    static void Main(string[] args) {
        // Gán phương thức Multiply cho delegate
        Operation operation = new Operation(Multiply);

        // Gọi delegate
        Console.WriteLine("Multiplication: " + operation(6, 3)); // Output: Multiplication: 18

        // Thay đổi delegate để trỏ tới phương thức Divide
        operation = new Operation(Divide);

        // Gọi lại delegate
        Console.WriteLine("Division: " + operation(9, 3)); // Output: Division: 3
    }
}
```
Trong ví dụ này, delegate `operation` được sử dụng để gọi cả phương thức `Multiply` và `Divide`. Điều này thể hiện tính linh hoạt của delegate khi bạn cần thay đổi phương thức được thực thi mà không cần sửa đổi mã nguồn.

## 3. Sử dụng Delegate để tạo Callback
Delegate đặc biệt hữu ích khi triển khai callback, cho phép truyền phương thức làm tham số để thực hiện sau đó.

Ví dụ: Triển khai Callback
```csharp
 
using System;

delegate void Notify(string message);

class Program
{
    static void Main(string[] args)
    {
        ProcessData("Processing data...", ShowMessage);
    }

    // Phương thức nhận delegate như một tham số (callback)
    static void ProcessData(string data, Notify callback)
    {
        Console.WriteLine("Data: " + data);
        callback("Data processed successfully!"); // Gọi callback
    }

    // Phương thức callback
    static void ShowMessage(string message)
    {
        Console.WriteLine("Callback message: " + message); // Output: Callback message: Data processed successfully!
    }
}
```
Trong ví dụ này, phương thức `ProcessData` nhận cả dữ liệu và một delegate làm tham số. Sau khi xử lý dữ liệu xong, nó sử dụng delegate để gọi phương thức `ShowMessage`, cho phép triển khai mẫu callback linh hoạt và mạnh mẽ.

## 4. Sử dụng Delegate với Lambda Expressions
Lambda expression cung cấp một cách ngắn gọn để biểu diễn các phương thức ẩn danh, có thể được sử dụng với delegate để mã nguồn trở nên dễ đọc và dễ bảo trì hơn.

Ví dụ: Lambda Expressions với Delegate
```csharp
 
using System;

delegate int Calculate(int x, int y);

class Program
{
    static void Main(string[] args)
    {
        // Sử dụng lambda expression cho phép cộng
        Calculate add = (x, y) => x + y;

        // Sử dụng lambda expression cho phép nhân
        Calculate multiply = (x, y) => x * y;

        int a = 10, b = 5;
        Console.WriteLine($"Sum of {a} and {b} is {add(a, b)}");      // Output: Sum of 10 and 5 is 15
        Console.WriteLine($"Product of {a} and {b} is {multiply(a, b)}"); // Output: Product of 10 and 5 is 50
    }
}
```
Ở đây, lambda expressions được sử dụng để định nghĩa hành vi của các delegate `add` và `multiply` một cách ngắn gọn và dễ đọc.

## 5. Delegate trong thực tế: Ví dụ thực tiễn
Để minh họa thêm về sức mạnh của delegate, hãy xem một tình huống thực tế nơi delegate cho phép thực hiện các chức năng động.

Ví dụ: Sắp xếp tùy chỉnh với Delegate
```csharp
 
using System;

delegate bool Compare(int x, int y);

class Program
{
    static void Main(string[] args)
    {
        int[] numbers = { 5, 2, 8, 3, 9 };

        // Sắp xếp tăng dần
        SortArray(numbers, (x, y) => x > y);

        Console.WriteLine("Sorted in Ascending Order:");
        foreach (var number in numbers)
        {
            Console.Write(number + " "); // Output: Sorted in Ascending Order: 2 3 5 8 9
        }
    }

    // Phương thức sắp xếp mảng sử dụng delegate để xác định thứ tự sắp xếp
    static void SortArray(int[] array, Compare compare)
    {
        for (int i = 0; i < array.Length - 1; i++)
        {
            for (int j = i + 1; j < array.Length; j++)
            {
                if (compare(array[i], array[j]))
                {
                    // Hoán đổi phần tử
                    int temp = array[i];
                    array[i] = array[j];
                    array[j] = temp;
                }
            }
        }
    }
}
```
Trong ví dụ này, phương thức `SortArray` sử dụng một delegate để xác định thứ tự sắp xếp. Điều này cho phép bạn truyền vào logic so sánh khác nhau, như sắp xếp tăng dần hay giảm dần, mà không cần thay đổi bản thân phương thức `SortArray`.
