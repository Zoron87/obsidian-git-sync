 Для создания строковой переменной используется следующая запись:

```go
 var str string = "Hello, World!"
```
или

```go
str := "Hello, World!"
```

или

```go
var str string
str = "Hello, World!"
```

Чтение до пробела:

```go
package main

import "fmt"

func main() {
    var s string
    fmt.Scan(&s) // Читает до пробела
    fmt.Println(s)
}
```

Или для чтения всей строки:

```go
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    scanner := bufio.NewScanner(os.Stdin)
    scanner.Scan()
    s := scanner.Text()
    fmt.Print(s)
}
```

### `strings.Contains`

Когда нужно проверить, **содержит ли строка другую строку**, начинается всё с `strings.Contains`. Эта функция возвращает `true`, если подстрока найдена, и `false` — если нет.
```go
import "strings" s := "IronProgrammer" fmt.Println(strings.Contains(s, "Program")) // true 
fmt.Println(strings.Contains(s, "Coder")) // false
```

### `strings.HasPrefix`

Если нужно проверить, **начинается ли строка с определённого слова**, используют `strings.HasPrefix`. А если нужно проверить окончание — `strings.HasSuffix`.
```go
fmt.Println(strings.HasPrefix("GoLang", "Go")) // true 
fmt.Println(strings.HasSuffix("GoLang", "Lang")) // true
```

### `strings.Split`

Когда строка содержит **несколько слов или значений, разделённых пробелами или другими символами**, её можно превратить в срез строк с помощью `strings.Split`.
```go
line := "яблоко,груша,банан" 
fruits := strings.Split(line, ",") 
fmt.Println(fruits) // [яблоко груша банан]
```