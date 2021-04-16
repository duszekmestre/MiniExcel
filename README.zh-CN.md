[![NuGet](https://img.shields.io/nuget/v/MiniExcel.svg)](https://www.nuget.org/packages/MiniExcel)  [![](https://img.shields.io/nuget/dt/MiniExcel.svg)](https://www.nuget.org/packages/MiniExcel)  [![Build status](https://ci.appveyor.com/api/projects/status/b2vustrwsuqx45f4/branch/master?svg=true)](https://ci.appveyor.com/project/shps951023/miniexcel/branch/master) [![.NET Framework](https://img.shields.io/badge/.NET%20Framework-%3E%3D%204.6.1-red.svg)](#)  [![.NET Standard](https://img.shields.io/badge/.NET%20Standard-%3E%3D%202.0-red.svg)](#) [![.NET](https://img.shields.io/badge/.NET%20-%3E%3D%205.0-red.svg)](#) 

---

[English](README.md) / [简体中文](README.zh-CN.md) / [繁體中文](README.zh-Hant.md)

----

QQ  : 459050029 (dotnet china) / 813100564  (MiniExcel讨论群)

---

### 简介

MiniExcel简单、高效避免OOM的.NET处理Excel查、写、填充数据工具。


目前主流框架大多需要将数据全载入到内存方便操作，但这会导致内存消耗问题，MiniExcel 尝试以 Stream 角度写底层算法逻辑，能让原本1000多MB占用降低到几MB，避免内存不够情况。

![image](https://user-images.githubusercontent.com/12729184/113084691-1804d000-9211-11eb-9b08-cbb89d9ecdc2.png)

### 特点
- 低内存耗用，避免OOM(out of memoery)、频繁 Full GC 情况
- 支持`即时`操作每行数据
![miniexcel_lazy_load](https://user-images.githubusercontent.com/12729184/111034290-e5588a80-844f-11eb-8c84-6fdb6fb8f403.gif)
- 兼具搭配 LINQ 延迟查询特性，能办到低消耗、快速分页等复杂查询  
- 轻量，不依赖任何套件，DLL小于100KB
- 简便操作的 API 风格



### Get Started

- [读 Excel](#getstart1)
- [写 Excel](#getstart2)
- [模板填充 Excel](#getstart3)
- [Excel Column Name/Index/Ignore Attribute](#getstart4)
- [范例](#getstart5)



### Demo
- LINQPad : Download [Basic Demo.linq](drafts/[MiniExcel]Basic%20Demo.linq)

### 安装

请查看 [from NuGet](https://www.nuget.org/packages/MiniExcel)

### 更新日志

请查看 [Release Notes](https://github.com/shps951023/MiniExcel/tree/master/docs)

### TODO 

请查看 [TODO](https://github.com/shps951023/MiniExcel/projects/1?fullscreen=true)

### 性能测试

以 [**Test1,000,000x10.xlsx**](https://github.com/shps951023/MiniExcel/blob/master/samples/xlsx/Test1%2C000%2C000x10/Test1%2C000%2C000x10.xlsx) 做基准与主流框架做性能测试，总共 1千万笔 "HelloWorld"，文件大小 23 MB   

Benchmarks  逻辑可以在 [MiniExcel.Benchmarks](https://github.com/shps951023/MiniExcel/tree/master/benchmarks/MiniExcel.Benchmarks) 查看或是提交 PR，运行指令

```
dotnet run -p .\benchmarks\MiniExcel.Benchmarks\ -c Release -f netcoreapp3.1 -- -f * --join
```

最后一次运行结果 :  

```
BenchmarkDotNet=v0.12.1, OS=Windows 10.0.19042
Intel Core i7-7700 CPU 3.60GHz (Kaby Lake), 1 CPU, 8 logical and 4 physical cores
  [Host]     : .NET Framework 4.8 (4.8.4341.0), X64 RyuJIT
  Job-ZYYABG : .NET Framework 4.8 (4.8.4341.0), X64 RyuJIT
IterationCount=3  LaunchCount=3  WarmupCount=3  
```

| Method                       | 最大内存耗用 |         平均时间 |        Gen 0 |       Gen 1 |      Gen 2 |
| ---------------------------- | -------------: | ---------------: | -----------: | ----------: | ---------: |
| 'MiniExcel QueryFirst'       |       0.109 MB |         726.4 us |            - |           - |          - |
| 'ExcelDataReader QueryFirst' |       15.24 MB |  10,664,238.2 us |  566000.0000 |   1000.0000 |          - |
| 'MiniExcel Query'            |        17.3 MB |  14,179,334.8 us |  367000.0000 |  96000.0000 |  7000.0000 |
| 'ExcelDataReader Query'      |        17.3 MB |  22,565,088.7 us | 1210000.0000 |   2000.0000 |          - |
| 'Epplus QueryFirst'          |       1,452 MB |  18,198,015.4 us |  535000.0000 | 132000.0000 |  9000.0000 |
| 'Epplus Query'               |       1,451 MB |  23,647,471.1 us | 1451000.0000 | 133000.0000 |  9000.0000 |
| 'OpenXmlSDK Query'           |       1,412 MB |  52,003,270.1 us |  978000.0000 | 353000.0000 | 11000.0000 |
| 'OpenXmlSDK QueryFirst'      |       1,413 MB |  52,348,659.1 us |  978000.0000 | 353000.0000 | 11000.0000 |
| 'ClosedXml QueryFirst'       |       2,158 MB |  66,188,979.6 us | 2156000.0000 | 575000.0000 |  9000.0000 |
| 'ClosedXml Query'            |       2,184 MB | 191,434,126.6 us | 2165000.0000 | 577000.0000 | 10000.0000 |


| Method                   | 最大内存耗用 |         平均时间 |        Gen 0 |        Gen 1 |      Gen 2 |
| ------------------------ | -------------: | ---------------: | -----------: | -----------: | ---------: |
| 'MiniExcel Create Xlsx'  |          15 MB |  11,531,819.8 us | 1020000.0000 |            - |          - |
| 'Epplus Create Xlsx'     |       1,204 MB |  22,509,717.7 us | 1370000.0000 |   60000.0000 | 30000.0000 |
| 'OpenXmlSdk Create Xlsx' |       2,621 MB |  42,473,998.9 us | 1370000.0000 |  460000.0000 | 50000.0000 |
| 'ClosedXml Create Xlsx'  |       7,141 MB | 140,939,928.6 us | 5520000.0000 | 1500000.0000 | 80000.0000 |



### 读 Excel <a name="getstart1"></a>



#### 1. Query 查询 Excel 返回`强型别` IEnumerable 数据 [[Try it]](https://dotnetfiddle.net/w5WD1J)

```C#
public class UserAccount
{
    public Guid ID { get; set; }
    public string Name { get; set; }
    public DateTime BoD { get; set; }
    public int Age { get; set; }
    public bool VIP { get; set; }
    public decimal Points { get; set; }
}

var rows = MiniExcel.Query<UserAccount>(path);

// or

using (var stream = File.OpenRead(path))
    var rows = stream.Query<UserAccount>();
```

![image](https://user-images.githubusercontent.com/12729184/111107423-c8c46b80-8591-11eb-982f-c97a2dafb379.png)


#### 2. Query 查询 Excel 返回`Dynamic` IEnumerable 数据 [[Try it]](https://dotnetfiddle.net/w5WD1J)

* Key 系统预设为 `A,B,C,D...Z`

| MiniExcel     | 1     |
| -------- | -------- |
| Github     | 2     |

```C#

var rows = MiniExcel.Query(path).ToList();

// or 
using (var stream = File.OpenRead(path))
{
    var rows = stream.Query().ToList();
                
    Assert.Equal("MiniExcel", rows[0].A);
    Assert.Equal(1, rows[0].B);
    Assert.Equal("Github", rows[1].A);
    Assert.Equal(2, rows[1].B);
}
```

#### 3. 查询数据以第一行数据当Key [[Try it]](https://dotnetfiddle.net/w5WD1J)

注意 : 同名以右边数据为准   

Input Excel :    

| Column1 | Column2 |
| -------- | -------- |
| MiniExcel     | 1     |
| Github     | 2     |


```C#

var rows = MiniExcel.Query(useHeaderRow:true).ToList();

// or

using (var stream = File.OpenRead(path))
{
    var rows = stream.Query(useHeaderRow:true).ToList();

    Assert.Equal("MiniExcel", rows[0].Column1);
    Assert.Equal(1, rows[0].Column2);
    Assert.Equal("Github", rows[1].Column1);
    Assert.Equal(2, rows[1].Column2);
}
```

#### 4. Query 查询支援延迟加载(Deferred Execution)，能配合LINQ First/Take/Skip办到低消耗、高效率复杂查询

举例 : 查询第一笔数据

```C#
var row = MiniExcel.Query(path).First();
Assert.Equal("HelloWorld", row.A);

// or

using (var stream = File.OpenRead(path))
{
    var row = stream.Query().First();
    Assert.Equal("HelloWorld", row.A);
}
```

与其他框架效率比较 :  

![queryfirst](https://user-images.githubusercontent.com/12729184/111072392-6037a900-8515-11eb-9693-5ce2dad1e460.gif)

#### 5. 查询指定 Sheet 名称

```C#
MiniExcel.Query(path, sheetName: "SheetName");
//or
stream.Query(sheetName: "SheetName");
```

#### 6. 查询所有 Sheet 名称跟数据

```C#
var sheetNames = MiniExcel.GetSheetNames(path).ToList();
foreach (var sheetName in sheetNames)
{
    var rows = MiniExcel.Query(path, sheetName: sheetName);
}
```

#### 7. 查询所有栏(列)

```C#
var columns = MiniExcel.GetColumns(path); // e.g result : ["A","B"...]

var cnt = columns.Count;  // get column count
```

#### 8. Dynamic Query 转成 `IDictionary<string,object>` 数据

```C#
foreach(IDictionary<string,object> row in MiniExcel.Query(path))
{
    //..
}
```





### 写 Excel  <a name="getstart2"></a>

1. 必须是非abstract 类别有公开无参数构造函数
2. MiniExcel SaveAs 支援 `IEnumerable参数延迟查询`，除非必要请不要使用 ToList 等方法读取全部数据到内存

图片 : 是否呼叫 ToList 的内存差别  

#### ![image](https://user-images.githubusercontent.com/12729184/112587389-752b0b00-8e38-11eb-8a52-cfb76c57e5eb.png)1. Anonymous or strongly type [[Try it]](https://dotnetfiddle.net/w5WD1J)

```C#
var path = Path.Combine(Path.GetTempPath(), $"{Guid.NewGuid()}.xlsx");
MiniExcel.SaveAs(path, new[] {
    new { Column1 = "MiniExcel", Column2 = 1 },
    new { Column1 = "Github", Column2 = 2}
});
```

#### 2. Datatable:  

```C#
var path = Path.Combine(Path.GetTempPath(), $"{Guid.NewGuid()}.xlsx");
var table = new DataTable();
{
    table.Columns.Add("Column1", typeof(string));
    table.Columns.Add("Column2", typeof(decimal));
    table.Rows.Add("MiniExcel", 1);
    table.Rows.Add("Github", 2);
}

MiniExcel.SaveAs(path, table);
```

#### 3. Dapper

```C#
using (var connection = GetConnection(connectionString))
{
    var rows = connection.Query(@"select 'MiniExcel' as Column1,1 as Column2 union all select 'Github',2");
    MiniExcel.SaveAs(path, rows);
}
```

#### 4. `IEnumerable<IDictionary<string, object>>`

```C#
var values = new List<Dictionary<string, object>>()
{
    new Dictionary<string,object>{{ "Column1", "MiniExcel" }, { "Column2", 1 } },
    new Dictionary<string,object>{{ "Column1", "Github" }, { "Column2", 2 } }
};
MiniExcel.SaveAs(path, values);
```

output : 

| Column1 | Column2 |
| -------- | -------- |
| MiniExcel     | 1     |
| Github     | 2     |

#### 5. SaveAs 支援 Stream [[Try it]](https://dotnetfiddle.net/JOen0e)

```C#
using (var stream = File.Create(path))
{
    stream.SaveAs(values);
}
```





### 模板填充 Excel <a name="getstart3"></a>

#### 1. 基本填充

模板:  
![image](https://user-images.githubusercontent.com/12729184/114537556-ed8d2b00-9c84-11eb-8303-a69f62c41e5b.png)

最终效果:  
![image](https://user-images.githubusercontent.com/12729184/114537490-d8180100-9c84-11eb-8c69-db58692f3a85.png)

代码:  
```C#
// 1. By POCO
var value = new
{
    Name = "Jack",
    CreateDate = new DateTime(2021, 01, 01),
    VIP = true,
    Points = 123
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);


// 2. By Dictionary
var value = new Dictionary<string, object>()
{
    ["Name"] = "Jack",
    ["CreateDate"] = new DateTime(2021, 01, 01),
    ["VIP"] = true,
    ["Points"] = 123
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```



#### 2. IEnumerable 数据填充

> Note1: 同行从左往右以第一个 IEnumerableUse 当列表来源 (不支持同列多集合)

模板:   
![image](https://user-images.githubusercontent.com/12729184/114564652-14f2f080-9ca3-11eb-831f-09e3fedbc5fc.png)

最终效果: 
![image](https://user-images.githubusercontent.com/12729184/114564204-b2015980-9ca2-11eb-900d-e21249f93f7c.png)

代码:

```C#
//1. By POCO
var value = new
{
    employees = new[] {
        new {name="Jack",department="HR"},
        new {name="Lisa",department="HR"},
        new {name="John",department="HR"},
        new {name="Mike",department="IT"},
        new {name="Neo",department="IT"},
        new {name="Loan",department="IT"}
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);

//2. By Dictionary
var value = new Dictionary<string, object>()
{
    ["employees"] = new[] {
        new {name="Jack",department="HR"},
        new {name="Lisa",department="HR"},
        new {name="John",department="HR"},
        new {name="Mike",department="IT"},
        new {name="Neo",department="IT"},
        new {name="Loan",department="IT"}
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```



#### 3. 复杂数据填充

> Note: 支持多 sheet 填充,并共用同一组参数

模板: 

![image](https://user-images.githubusercontent.com/12729184/114565255-acf0da00-9ca3-11eb-8a7f-8131b2265ae8.png)

最终效果: 

![image](https://user-images.githubusercontent.com/12729184/114565329-bf6b1380-9ca3-11eb-85e3-3969e8bf6378.png)

代码:  

```C#
// 1. By POCO
var value = new
{
    title = "FooCompany",
    managers = new[] {
        new {name="Jack",department="HR"},
        new {name="Loan",department="IT"}
    },
    employees = new[] {
        new {name="Wade",department="HR"},
        new {name="Felix",department="HR"},
        new {name="Eric",department="IT"},
        new {name="Keaton",department="IT"}
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);

// 2. By Dictionary
var value = new Dictionary<string, object>()
{
    ["title"] = "FooCompany",
    ["managers"] = new[] {
        new {name="Jack",department="HR"},
        new {name="Loan",department="IT"}
    },
    ["employees"] = new[] {
        new {name="Wade",department="HR"},
        new {name="Felix",department="HR"},
        new {name="Eric",department="IT"},
        new {name="Keaton",department="IT"}
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```

#### 4. 大数据填充效率比较

> NOTE: 在 MiniExcel 使用 IEnumerable 延迟 ( 不ToList ) 可以节省内存使用

![image](https://user-images.githubusercontent.com/12729184/114577091-5046ec80-9cae-11eb-924b-087c7becf8da.png)

#### 5. Cell 值自动类别对应

模板

![image](https://user-images.githubusercontent.com/12729184/114802504-64830a80-9dd0-11eb-8d56-8e8c401b3ace.png)

最终效果

![image](https://user-images.githubusercontent.com/12729184/114802419-43221e80-9dd0-11eb-9ffe-a2ce34fe7076.png)

类别

```C#
public class Poco
{
    public string @string { get; set; }
    public int? @int { get; set; }
    public decimal? @decimal { get; set; }
    public double? @double { get; set; }
    public DateTime? datetime { get; set; }
    public bool? @bool { get; set; }
    public Guid? Guid { get; set; }
}
```

代码

```C#
var poco = new TestIEnumerableTypePoco { @string = "string", @int = 123, @decimal = decimal.Parse("123.45"), @double = (double)123.33, @datetime = new DateTime(2021, 4, 1), @bool = true, @Guid = Guid.NewGuid() };
var value = new
{
    Ts = new[] {
        poco,
        new TestIEnumerableTypePoco{},
        null,
        poco
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```



#### 6. Example :  列出 Github 专案

模板

![image-20210415103138835](https://i.loli.net/2021/04/15/KWIcwatZTEpDPjJ.png)

最终效果

![image-20210415103052651](https://i.loli.net/2021/04/15/wMTBoJyNDspnZqh.png)

代码

```C#
var projects = new[]
{
    new {Name = "MiniExcel",Link="https://github.com/shps951023/MiniExcel",Star=146, CreateTime=new DateTime(2021,03,01)},
    new {Name = "HtmlTableHelper",Link="https://github.com/shps951023/HtmlTableHelper",Star=16, CreateTime=new DateTime(2020,02,01)},
    new {Name = "PocoClassGenerator",Link="https://github.com/shps951023/PocoClassGenerator",Star=16, CreateTime=new DateTime(2019,03,17)}
};
var value = new
{
    User = "ITWeiHan",
    Projects = projects,
    TotalStar = projects.Sum(s => s.Star)
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```







### Excel Column Name/Index/Ignore Attribute <a name="getstart4"></a>

e.g

input excel :  

![image](https://user-images.githubusercontent.com/12729184/114230869-3e163700-99ac-11eb-9a90-2039d4b4b313.png)

```C#
public class ExcelAttributeDemo
{
    [ExcelColumnName("Column1")]
    public string Test1 { get; set; }
    [ExcelColumnName("Column2")]
    public string Test2 { get; set; }
    [ExcelIgnore]
    public string Test3 { get; set; }
    [ExcelColumnIndex("I")] // system will convert "I" to 8 index
    public string Test4 { get; set; } 
    public string Test5 { get; } //wihout set will ignore
    public string Test6 { get; private set; } //un-public set will ignore
    [ExcelColumnIndex(3)] // start with 0
    public string Test7 { get; set; }
}

var rows = MiniExcel.Query<ExcelAttributeDemo>(path).ToList();
Assert.Equal("Column1", rows[0].Test1);
Assert.Equal("Column2", rows[0].Test2);
Assert.Null(rows[0].Test3);
Assert.Equal("Test7", rows[0].Test4);
Assert.Null(rows[0].Test5);
Assert.Null(rows[0].Test6);
Assert.Equal("Test4", rows[0].Test7);
```



### Excel 类别自动判断 <a name="getstart5"></a>

MiniExcel 预设会根据扩展名或是 Stream 类别判断是 xlsx 还是 csv，但会有失准时候，请自行指定。

```C#
stream.SaveAs(excelType:ExcelType.CSV);
//or
stream.SaveAs(excelType:ExcelType.XLSX);
//or
stream.Query(excelType:ExcelType.CSV);
//or
stream.Query(excelType:ExcelType.XLSX);
```



### 范例

#### 1. SQLite & Dapper 读取大数据新增到数据库

note : 请不要呼叫 call ToList/ToArray 等方法，这会将所有数据读到内存内

```C#
using (var connection = new SQLiteConnection(connectionString))
{
    connection.Open();
    using (var transaction = connection.BeginTransaction())
    using (var stream = File.OpenRead(path))
    {
       var rows = stream.Query();
       foreach (var row in rows)
             connection.Execute("insert into T (A,B) values (@A,@B)", new { row.A, row.B }, transaction: transaction);
       transaction.Commit();
    }
}
```

效能:
![image](https://user-images.githubusercontent.com/12729184/111072579-2dda7b80-8516-11eb-9843-c01a1edc88ec.png)


#### 2. ASP.NET Core 3.1 or MVC 5 下载 Excel Xlsx API Demo

```C#
public class ExcelController : Controller
{
    public IActionResult Download()
    {
        var values = new[] {
            new { Column1 = "MiniExcel", Column2 = 1 },
            new { Column1 = "Github", Column2 = 2}
        };
        var stream = new MemoryStream();
        stream.SaveAs(values);
        return File(stream,
            "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
            "demo.xlsx");
    }
}
```

####  3. 分页查询

```C#
void Main()
{
	var rows = MiniExcel.Query(path);
	
	Console.WriteLine("==== No.1 Page ====");
	Console.WriteLine(Page(rows,pageSize:3,page:1));
	Console.WriteLine("==== No.50 Page ====");
	Console.WriteLine(Page(rows,pageSize:3,page:50));
	Console.WriteLine("==== No.5000 Page ====");
	Console.WriteLine(Page(rows,pageSize:3,page:5000));
}

public static IEnumerable<T> Page<T>(IEnumerable<T> en, int pageSize, int page)
{
	return en.Skip(page * pageSize).Take(pageSize);
}
```

![20210419](https://user-images.githubusercontent.com/12729184/114679083-6ef4c400-9d3e-11eb-9f78-a86daa45fe46.gif)



### FQA

#### Q: 如何将查询结果转为 DataTable

提醒 : 不建议使用，因为DataTable会将数据`全载入内存`，失去MiniExcel低内存消耗功能。

```C#
public static DataTable QueryAsDataTable(string path)
{
	var rows = MiniExcel.Query(path, true);
	var dt = new DataTable();
	var first = true;
	foreach (IDictionary<string, object> row in rows)
	{
		if (first)
		{
			foreach (var key in row.Keys)
			{
				var type = row[key]?.GetType() ?? typeof(string);
				dt.Columns.Add(key, type);
			}

			first = false;
		}
		dt.Rows.Add(row.Values.ToArray());
	}
	return dt;
}
```

![image-20210415120352604](https://i.loli.net/2021/04/15/TbtP1GLDEgONueK.png)



### 局限与警告

- 目前不支援 xls (97-2003) 或是加密文件。



### 参考
- 读取逻辑 :  [ExcelDataReader](https://github.com/ExcelDataReader/ExcelDataReader)  / [ClosedXML](https://github.com/ClosedXML/ClosedXML)
- API 设计方式 :　[StackExchange/Dapper](https://github.com/StackExchange/Dapper)    

### Contributors  

![](https://contrib.rocks/image?repo=shps951023/MiniExcel)