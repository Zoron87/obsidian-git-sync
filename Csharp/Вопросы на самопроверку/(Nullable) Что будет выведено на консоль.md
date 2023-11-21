
```C#
static void NullableTest() 
{ 
int? a = null; 
object aObj = a; 

int? b = new int?(); 
object bObj = b; 

Console.WriteLine(Object.ReferenceEquals(aObj, bObj)); // True or False? 
}
```