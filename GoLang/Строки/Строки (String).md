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