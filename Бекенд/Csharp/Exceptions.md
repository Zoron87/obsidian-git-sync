1. **Ошибки аргументов и контрактов**

- **ArgumentException**, **ArgumentNullException**, **ArgumentOutOfRangeException** — используются для обработки ошибок, связанных с некорректными входными параметрами.
    
- Например, если переданный аргумент имеет неверный формат или значение выходит за пределы допустимого диапазона.
```csharp
if (value < 0)
    throw new ArgumentOutOfRangeException(nameof(value), "Значение не может быть отрицательным.");

```