# C# CSV Serializer

This C# class giving a simplest way with performance serializer functionalities to manipulate CSV strings; several test cases were executed in order to treat the particularities of a sequence of CSV characters; in this publication, is considered stable, it is implemented in productive environments with large scale of data generated or consumed in CSV daily.

Specifications: This 'CsvSerializer' class enjoy best pratices of Design Patterns result in a powerful CSV serialization and deserialization functionality, giving to Developer other data sctructure options like [DataTable](https://docs.microsoft.com/en-us/dotnet/api/system.data.datatable) object or a custom C# Plain Old CLR Object (Entity class) with capacity to define column name and order exihbition.
Both methods 'Serialize' and 'Deserialize' contains on important argument named "csvSeparator" to gives flexible generation of CSV.

Bellow are examples of using the 'CsvSerializer':

## 1) Serialize

#### 1.1) Using a POCO (Plain Old CLR Objects) class, with native .NET [DataMemberAtribute](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.serialization.datamemberattribute) (attribute is not mandatory):
```cs
public class ExampleModel
{
    [DataMember(Name = "My Property 1", Order = 4)]
    public string MyProperty1 { get; set; }

    [DataMember(Name = "My Property 2", Order = 1)]
    public string MyProperty2 { get; set; }

    [DataMember(Name = "Any Property", Order = 3)]
    public string AnyProperty { get; set; }

    [DataMember(Name = "Property comments change order", Order = 6)]
    public string PropertyWithChangeOrder { get; set; }

    [DataMember(Name = "Some Property", Order = 2)]
    public string SomeProperty { get; set; }

    [DataMember(Name = "Capacity Property", Order = 5)]
    public string CapacityProperty { get; set; }
}
```

#### 1.2) Create a collection based on Entity class above:
```cs
List<ExampleModel> list = new List<ExampleModel>();

list.Add(new ExampleModel()
{
    AnyProperty = "any 1",
    CapacityProperty = "20",
    MyProperty1 = "my 1",
    MyProperty2 = "my 2",
    PropertyWithChangeOrder = "Comments to change and or texts...",
    SomeProperty = "some 1"
});
list.Add(new ExampleModel()
{
    AnyProperty = "any yna",
    CapacityProperty = "45",
    MyProperty1 = "11 my",
    MyProperty2 = "22 my",
    PropertyWithChangeOrder = "Another comment to change contents!",
    SomeProperty = "2 some 1"
});
list.Add(new ExampleModel()
{
    AnyProperty = @"\/any yna\/",
    CapacityProperty = "57",
    MyProperty1 = "1my1",
    MyProperty2 = "2my2",
    PropertyWithChangeOrder = "Texts with comments to rich all tests",
    SomeProperty = "some3"
});
```

#### 1.3) Execute Serialize method, and *Voilà*!
```cs
string csvSerialized = CsvSerializer<ExampleModel>.Serialize(';', list);
```

#### 1.4) This is the result of the string bulk:
```csv
My Property 2;Some Property;Any Property;My Property 1;Capacity Property;Property comments change order;

my 2;some 1;any 1;my 1;20;Comments to change and or texts...;

22 my;2 some 1;any yna;11 my;45;Another comment to change contents!;

2my2;some3;\/any yna\/;1my1;57;Texts with comments to rich all tests;
```

#### 1.5) Pear attention: if you want to save '*.csv*' file, remeber to use Encoding UTF8, according [CSV specifications](https://en.wikipedia.org/wiki/Comma-separated_values):
```cs
File.WriteAllText(@"\\path\to\save\file.csv", csvSerialized, Encoding.UTF8);
```
----------------------------

## 2) Deserialize

#### 2.1) The developer may consume any '*.csv*' file by sugestted instruction:
```cs
string csvToDeserialize = File.ReadAllText(@"\\path\to\read\file.csv");
```

#### 2.2) And, if dont know the data column arrange, may populate a DataTable objet:
```cs
DataTable dtDeserialized = CsvSerializer.Deserialize(';', csvToDeserialize);
```

#### 2.2) Or populate a collection of Entity class (like array or list):
```cs
ExampleModel[] arrayCollection = CsvSerializer<ExampleModel>.Deserialize(';', csvToDeserialize).ToArray();
List<ExampleModel> listCollection = CsvSerializer<ExampleModel>.Deserialize(';', csvToDeserialize).ToList();
```
----------------------
## Installing

You can start using this project with next steps:

1. Clone as [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

2. Just [add](https://docs.microsoft.com/en-us/visualstudio/ide/managing-references-in-a-project) following references:
    1. ```System.Data```
    2. ```System.Data.DataSetExtensions```
    3. ```System.Data.Entity```
    4. ```System.Data.Linq```
    5. ```System.Runtime.Serialization```

3. Have fun!
---

## License

[View MIT license](https://github.com/antonio-leonardo/CsvSerializer/blob/master/LICENSE)

## Main Project

[View Link](https://github.com/antonio-leonardo/CsvSerializer)

## Project Link

[Link](https://github.com/DenisBelobrotski/CsvSerializer)

---
## Fork Info

Differences with main repository:
1. This [issue](https://github.com/antonio-leonardo/CsvSerializer/issues/1) fixed.
2. Quotes supporting added (with this [regular expression](https://stackoverflow.com/questions/18144431/regex-to-split-a-csv)). This example isn't parsing correctly in main repository:
```
"1","Hey, Soul Sister","Train","neo mellow",2010,97,89,67,-4,8,80
```
3. Fixed skipped last element in row.
4. Content reading optimized a little bit.

Nearest plans:
1. Compare order of DataMember attribute to optimize attribute searching.

Known issues:
1. The following line isn't parsing correctly with current regular expression because of double quotes:
```
"203","Ooh La La (from ""The Smurfs 2"")","Britney Spears","dance pop",2013,128,57,69
```
