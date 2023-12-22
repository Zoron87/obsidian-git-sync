![[Pasted image 20231223012126.png]]

Воспользуйтесь методом Enumerable.SequenceEqual.

SequenceEqual<TSource>(IEnumerable<TSource>, IEnumerable<TSource>) 
Определяет, совпадают ли две последовательности, используя для сравнения элементов компаратор проверки на равенство по умолчанию, предназначенный для их типа. C# 

public static bool SequenceEqual<TSource> (this System.Collections.Generic.IEnumerable<TSource> first, System.Collections.Generic.IEnumerable<TSource> second);

#### Возвращаемое значение

`true`, если у двух исходных последовательностей одинаковая длина и соответствующие элементы совпадают, согласно компаратору проверки на равенство по умолчанию для этого типа элементов, в противном случае — `false`.