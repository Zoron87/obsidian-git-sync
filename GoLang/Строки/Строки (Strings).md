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

### `strings.Fields`

Если разделитель — **пробел**, и при этом между словами может быть несколько пробелов, лучше использовать `strings.Fields`. Эта функция сама разобьёт строку на слова, игнорируя лишние пробелы.

```go
s := "  Я  люблю   Go  "
words := strings.Fields(s)
fmt.Println(words) // [Я люблю Go]
```

### Замена подстрок

Если нужно **заменить часть строки на другую**, используют `strings.Replace`. Она позволяет указать, сколько замен нужно сделать. Если нужно заменить **все вхождения**, используют `strings.ReplaceAll`.

```go
s := "мяу мяу мяу"
newS := strings.Replace(s, "мяу", "гав", 2)
fmt.Println(newS) // гав гав мяу

all := strings.ReplaceAll(s, "мяу", "гав")
fmt.Println(all) // гав гав гав
```

### Изменение регистра:

### `strings.ToLower` и `strings.ToUpper`

Для приведения строки к **нижнему или верхнему регистру** используют `ToLower` и `ToUpper`. Это полезно, например, при сравнении строк без учёта регистра.

```less
fmt.Println(strings.ToLower("GoLang")) // golang
fmt.Println(strings.ToUpper("GoLang")) // GOLANG
```

### `strings.Title`

Функция `strings.Title` делает первую букву каждого слова заглавной. Однако она работает корректно только с латиницей и считается устаревшей — её лучше избегать в серьёзных проектах.

### Поиск позиции

Если нужно узнать, **на какой позиции находится подстрока**, используют `strings.Index`. Если подстрока не найдена — возвращается `-1`.

```css
s := "IronProgrammer"
fmt.Println(strings.Index(s, "Pro"))  // 4
fmt.Println(strings.Index(s, "Java")) // -1
```

 `strings.LastIndex` ищет **последнее вхождение** подстроки.

### Удаление пробелов:

Когда строка приходит с лишними пробелами или символами в начале и конце, используют `TrimSpace` — он удаляет все пробелы, табуляции и переводы строк с краёв.

```css
s := "   привет   "
clean := strings.TrimSpace(s)
fmt.Println(clean) // "привет"
```

Если нужно удалить **конкретный символ** (например, запятую или точку), используют `Trim`, `TrimPrefix` или `TrimSuffix`.

```css
s := ">>>важно<<<"
clean := strings.Trim(s, "<>")
fmt.Println(clean) // "важно"
```

### Сравнение строк без учёта регистра

`Go` различает регистр, поэтому `"Go"` и `"go"` — это разные строки. Чтобы сравнить их **без учёта регистра**, нужно привести обе к одному регистру:

```css
a := "GoLang"
b := "golang"

equal := strings.ToLower(a) == strings.ToLower(b)
fmt.Println(equal) // true
```