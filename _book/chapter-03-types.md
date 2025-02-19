---
title:  "Типы"
layout: "chapter"

---

В предыдущей главе мы использовали строковый тип данных, чтобы хранить 
`Hello World`. Типы данных определяют множество принимаемых значений, описывают, 
какие операции могут быть применены к ним, и определяют, как данные будут храниться.
Поскольку типы данных могут быть сложны для понимания, мы попробуем рассмотреть
их подробнее, прежде чем разбираться, как они реализованы в Go.

Предположим,  у вас есть собака по имени Шарик. Тут «Шарик» — это «Собака», этот тип
описывает какой-то набор свойств, присущий всем собакам. Наши рассуждения должны
быть примерно следующие: у собак 4 лапы, Шарик — собака, значит, у Шарика 4 лапы.
Типы данных в языках программирования работают похожим образом: у всех строк есть 
длина; `x` — строка, а значит у `x` есть длина.

В математике мы часто говорим о множествах. Например, `ℝ` (множество всех
вещественных чисел) или `ℕ` (множество всех натуральных чисел). Каждый элемент
этих множеств имеет такие же свойства, как и все прочие элементы этого
множества. Например, все натуральные числа ассоциативны - «для всех натуральных
чисел a, b и c выполняется: `a + (b + c) = (a + b) + c` и `a × (b × c) = (a × b) ×
c`»; в этом смысле множества схожи с типами данных в языках программирования тем, 
что все значения одного типа имеют общие свойства.

Go — это язык программирования со статической типизацией. Это означает, что
переменные всегда имеют определенный тип и этот тип нельзя изменить. Статическая
типизация, на первый взгляд, может показаться неудобной. Вы потратите кучу
времени только на попытки исправить ошибки, не позволяющие программе
скомпилироваться. Однако типы дают вам возможность понять, что именно делает
программа, и помогают избежать распространённых ошибок.

В Go есть несколько встроенных типов данных, с которыми мы сейчас ознакомимся.

## Числа

В Go есть несколько различных типов для представления чисел. Вообще, мы разделим
числа на два различных класса: целые числа и числа с плавающей точкой.

### Целые числа

Целые числа, точно так же, как их математические коллеги, — это числа без
вещественной части. В отличие от десятичного представления чисел, которое
используем мы, компьютеры используют двоичное представление.

Наша система строится на 10 различных цифрах. Когда мы исчерпываем доступные нам
цифры, мы представляем большое число, используя новую цифру 2 (а затем 3, 4, 5, …)
числа следуют одно за другим. Например, число, следующее за 9, это 10, число,
следующее за 99, это 100 и так далее. Компьютеры делают то же самое, но они имеют
только 2 цифры вместо 10. Поэтому, подсчет выглядит так: 0, 1, 10, 11, 100, 101,
110, 111 и так далее. Другое отличие между той системой счисления, что используем
мы, и той, что использует компьютер - все типы чисел имеют строго определенный
размер. У них есть ограниченное количество цифр. Поэтому четырехразрядное число
может выглядеть так: 0000, 0001, 0010, 0011, 0100. В конце концов мы можем
выйти за лимит, и большинство компьютеров просто вернутся к самому началу (что
может стать причиной очень странного поведения программы).

В Go существуют следующие типы целых чисел: `uint8`, `uint16`, `uint32`,
`uint64`, `int8`, `int16`, `int32` и `int64`. 8, 16, 32 и 64 говорит нам,
сколько бит использует каждый тип. `uint` означает «unsigned integer»
(беззнаковое целое), в то время как `int` означает «signed integer» (знаковое
целое). Беззнаковое целое может принимать только положительные значения (или
ноль).

|Тип    |Значение                               |Диапазон                                      |
|:------|:--------------------------------------|:---------------------------------------------|
|uint8  |Беззнаковые 8-битные целые числа       |от 0 до 255                                   |
|uint16 |Беззнаковые 16-битные целые числа      |от 0 до 65535                                 |
|uint32 |Беззнаковые 32-битные целые числа      |от 0 до 4294967295                            |
|uint64 |Беззнаковые 64-битные целые числа      |от 0 до 18446744073709551615                  |
|int8   |Знаковые 8-битные целые числа          |от -128 до 127                                |
|int16  |Знаковые 16-битные целые числа         |от -32768 до 32767                            |
|int32  |Знаковые 32-битные целые числа         |от -2147483648 до 2147483647                  |
|int64  |Знаковые 64-битные целые числа         |от -9223372036854775808 до 9223372036854775807|

В дополнение к этому существуют два типа-псевдонима: `byte` (то же
самое, что `uint8`) и `rune` (то же самое, что `int32`). Байты — очень
распространенная единица измерения в компьютерах (1 байт = 8 бит, 1024 байта = 1
килобайт, 1024 килобайта = 1 мегабайт, …), и именно поэтому тип `byte` в Go часто
используется для определения других типов. Также существует 3 машинно-зависимых
целочисленных типа: `uint`, `int` и `uintptr`. Они машинно-зависимы, потому что
их размер зависит от архитектуры используемого компьютера.

В общем, если вы работаете с целыми числами — просто используйте тип `int`.

## Числа с плавающей точкой 

Числа с плавающей точкой — это числа, которые содержат вещественную часть
(вещественные числа) (1.234, 123.4, 0.00001234, 12340000). Их представление в
компьютере довольно сложно и не особо необходимо для их использования. Так что мы
просто должны помнить:

*   Числа с плавающей точкой неточны. Бывают случаи, когда число вообще нельзя
    представить. Например, результатом вычисления `1.01 - 0.99` будет
    `0.020000000000000018` - число очень близкое к ожидаемому, но не то же самое.

*   Как и целые числа, числа с плавающей точкой имеют определенный размер (32 бита
    или 64 бита). Использование большего размера увеличивает точность (сколько цифр
    мы можем использовать для вычисления)

*   В дополнение к числам существуют несколько других значений, таких как: «not a
    number» (не число) (`NaN`, для вещей наподобие `0/0`), а также положительная и
    отрицательная бесконечность (`+∞` и `−∞`).

В Go есть два вещественных типа: `float32` и `float64` (соответственно, часто
называемые вещественными числами с одинарной  и двойной точностью). А также два
дополнительных типа для представления комплексных чисел (чисел с мнимой частью):
`complex64` и `complex128`. Как правило, мы должны придерживаться типа `float64`,
когда работаем с числами с плавающей точкой.

### Пример

Давайте напишем программу-пример, использующую числа. Во-первых, создайте папку
«chapter3» с файлом main.go внутри со следующим содержимым:

    package main

    import "fmt"

    func main() {
        fmt.Println("1 + 1 = ", 1 + 1)
    }

Если вы запустите программу, то должны увидеть это:

    $ go run main.go
    1 + 1 = 2

Заметим, что эта программа очень схожа с программой, которую мы написали в главе 2.
Она содержит ту же строку с указанием пакета, ту же строку с импортом, то же
определение функции и использует ту же функцию `Println`. В этот раз вместо
печати строки `Hello World` мы печатаем строку `1 + 1 = ` с последующим
результатом выражения `1 + 1`. Это выражение состоит из трех частей: числового
литерала `1` (который является типом `int`), оператора `+` (который представляет
сложение) и другого числового литерала `1`. Давайте попробуем сделать то же
самое, используя числа с плавающей точкой:

    fmt.Println("1 + 1 =", 1.0 + 1.0)

Обратите внимание, что мы используем `.0`, чтобы сказать Go, что это число с
плавающей точкой, а не целое. При выполнении этой программы результат будет тот
же, что и прежде.

В дополнение к сложению, в Go имеется несколько других операций:

| Литерал | Пояснение          |
|---------|--------------------|
| +       | сложение           |
| -       | вычитание          |
| *       | умножение          |
| /       | деление            |
| %       | остаток от деления |

## Строки

Как мы видели в главе 2, строка — это последовательность символов определенной
длины, используемая для представления текста. Строки в Go состоят из независимых
байтов, обычно по одному на каждый символ (символы из других языков, таких как
китайский, представляются несколькими байтами).

Строковые литералы могут быть созданы с помощью двойных кавычек `"Hello World"`
или с помощью апострофов `` `Hello World` ``. Различие между ними в том, что
строки в двойных кавычках не могут содержать новые строки и они позволяют
использовать особые управляющие последовательности символов. Например, `\n` будет
заменена символом новой строки, а `\t` - символом табуляции.

Распространенные операции над строками включают в себя нахождение длины строки
`len("Hello World")`, доступ к отдельному символу в строке `"Hello World"[1]`, и
конкатенацию двух строк `"Hello " + "World"`. Давайте модифицируем
созданную ранее программу, чтобы проверить всё это:

    package main
    
    import "fmt"

    func main() {
        fmt.Println(len("Hello World"))
        fmt.Println("Hello World"[1])
        fmt.Println("Hello " + "World")
    }

На заметку:

*   Пробел тоже считается символом, поэтому длина строки 11 символов, а не 10 и
    третья строка содержит `"Hello "` вместо `"Hello"`.

*   Строки “индексируются” начиная с 0, а не с 1. [1] даст вам второй элемент, а не
    первый. Также заметьте, что вы видите `101` вместо `e`, когда выполняете
    программу. Это происходит из-за того, что символ представляется байтом (помните,
    байт — это целое число).

    Можно думать об индексации так: `"Hello World"` `1`. Читайте это так: «строка
    Hello World позиция 1», «на 1 позиции строки Hello World» или «второй символ
    строки Hello World».

*   Конкатенация использует тот же символ, что и сложение. Компилятор Go выясняет,
    что должно происходить, полагаясь на типы аргументов. Если по обе стороны от `+`
    находятся строки, компилятор предположит, что вы имели в виду конкатенацию, а не
    сложение (ведь сложение для строк бессмысленно).

## Логические типы

Булевский тип (названный так в честь Джорджа Буля) — это специальный однобитный
целочисленный тип, используемый для представления истинности и ложности. С этим типом
используются три логических оператора:

| Литерал      | Пояснение |
|--------------|-----------|
| &&           | И         |
| &#124;&#124; | ИЛИ       |
| !            | НЕ        |

Вот пример программы, показывающей их использование:

    func main() {
        fmt.Println(true && true)
        fmt.Println(true && false)
        fmt.Println(true || true)
        fmt.Println(true || false)
        fmt.Println(!true)
    }

Запуск этой программы должен вывести:

    $ go run main.go
    true
    false
    true
    true
    false

Используем таблицы истинности, чтобы определить, как эти операторы работают: 

| Выражение      | Значение |
|----------------|----------|
| true && true   | true     |
| true && false  | false    |
| false && true  | false    |
| false && false | false    |

| Выражение                | Значение |
|--------------------------|----------|
| true &#124;&#124; true   | true     |
| true &#124;&#124; false  | true     |
| false &#124;&#124; true  | true     |
| false &#124;&#124; false | false    |


| Выражение | Значение |
|-----------|----------|
| !true     | false    |
| !false    | true     |

Всё это — простейшие типы, включенные в Go и являющиеся основой, с помощью
которой строятся все остальные типы.

## Задачи

*   Как хранятся числа в компьютере?

*   Мы знаем, что в десятичной системе самое большое число из одной цифры - это 9, а
    из двух - 99. В бинарной системе самое большое число из
    двух цифр это 11 (3), самое большое число из трех цифр это 111 (7) и самое
    большое число из 4 цифр это 1111 (15). Вопрос: каково самое большое число из 8
    цифр? (Подсказка: 101-1=9 и 102-1=99)

*   В зависимости от задачи вы можете использовать Go как калькулятор. Напишите
    программу, которая вычисляет `32132  × 42452` и печатает это в терминал
    (используйте оператор `*` для умножения).

*   Что такое строка? Как найти её длину?

*   Какое значение примет выражение 
    `(true && false) || (false && true) || !(false && false)`?
