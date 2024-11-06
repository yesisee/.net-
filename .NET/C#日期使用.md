### 1.获取当月的开始日期和当月的结束日期
```C#
using System;

class Program
{
    static void Main()
    {
        // 获取当前日期
        DateTime now = DateTime.Now;

        // 获取当前月份的开始时间
        DateTime startOfMonth = new DateTime(now.Year, now.Month, 1);

        // 获取当前月份的结束时间
        DateTime endOfMonth = startOfMonth.AddMonths(1).AddDays(-1).Date.AddDays(1).AddTicks(-1);

        // 输出结果
        Console.WriteLine("开始时间: " + startOfMonth);
        Console.WriteLine("结束时间: " + endOfMonth);
    }
}

```
