## Безымянные переменные (Unnamed Patterns and Variables) (java 21+)

Безымянные переменные, обозначаются символом `_` (подчеркивание)  

```java
int acc = 0;
for (Order _ : orders) {
    if (acc < LIMIT) {
        … acc++ …
    }
}
```

В примере важен факт наличия переменное, но ее имя можно не придумывать, потому что сама переменная не нужна  
Аналогичное использование: безымянные try-catch. Пример:

```java
String s = // ...
try {
    int i = Integer.parseInt(s);
    // ....
} catch (NumberFormatException _) {
    System.out.println("Bad number: " + s);
}
```

Случаи в которых можно использовать безымянные переменные: 
* Локальная переменная в блоке
* Объявление ресурса в `try-with-resources`
* Заголовок `for` statement
* Заголовок улучшенного цикла `for`
* Исключение в блоке `catch`
* Параметр лямбда-выражения
* Переменная паттерна (см ниже)

Параметры методов не могут быть безымянными

Безымянные паттерны, пример:
```java
if (r instanceof ColoredPoint(Point(int x, int y), _)) {
    // Используются только x и y
}
```

Класс `ColoredPoint` содержит точку (`Point`) и цвет (`Color`)  
Разработчику нужны только координаты x, y, а цвет в данном случае не важен, поэтому его можно указать 
через безымянную переменную  
Если требуется цвет, то код будет выглядеть так:
```java
if (r instanceof ColoredPoint(Point(int x, int y), Color c)) {
    // Используются x, y и цвет
}
```
Если `Color c` не используется, то получим `Warning: unused c`  

Аналогичный безымянные паттерн, с указание типа:
```java
if (r instanceof ColoredPoint(Point(int x, int y), Color _)) {
    // ...
}
```
Мы указали класс `Color`, но саму переменную оставили безымянной

Безымянные паттерны сочетаются с конструкцией `switch`, пример:
```java
switch (box) {
    case Box(RedBall _), Box(BlueBall _) -> processBox(box);
    case Box(GreenBall _)                -> stopProcessing();
    case Box(_)                          -> pickAnotherBox();
}
```

---

Дополнительные примеры использования:
```java
... instanceof Point(int x, _)
```

```java
case Point(int x, _)
```

```java
int _ = q.remove();
```

```java
} catch (NumberFormatException _) {
```
```java
(int x,int _)->x+x
```


---

## Безымянные классы (Unnamed Classes and Instance Main Methods) (java 21+)

Теперь в режиме preview можно запускать программы с методами `main()`,
которые не являются `public static` и у которых нет параметра `String[] args`.  
В таком случае JVM сама создаст экземпляр класса 
(у него должен быть не-private конструктор без параметров) и вызовет у него метод `main()`.  

```java
class HelloWorld {
    void main() {
        System.out.println("Hello, World!");
    }
}
```

Протокол запуска будет выбирать метод `main()` согласно следующему приоритету:

1. `static void main(String[] args)`
2. `static void main()`
3. `void main(String[] args)`
4. `void main()`

Кроме того, можно писать программы и без объявления класса вовсе:
```java
String greeting = "Hello, World!";

void main() {
    System.out.println(greeting);
}
```

В таком случае будет создан неявный безымянный класс (не путать с анонимным классом),
которому будут принадлежать метод `main()` и другие верхнеуровневые объявления в файле:
```java
// class <some name> { ← неявно
String greeting = "Hello, World!";

void main() {
    System.out.println(greeting);
}
// }
```

Безымянный класс является синтетическим и `final`. Его simple name является пустой строкой:
```java
void main() {
    System.out.println(getClass().isUnnamed()); // true
    System.out.println(getClass().isSynthetic()); // true
    System.out.println(getClass().getSimpleName()); // ""
    System.out.println(getClass().getCanonicalName()); // null
}
```

При этом имя класса совпадает с именем файла, но такое поведение не гарантируется.


---

Ссылки:
- JEP-443, Unnamed Patterns and Variables: https://openjdk.org/jeps/443
- JEP-445, Unnamed Classes and Instance Main Methods: https://openjdk.org/jeps/445
- https://habr.com/ru/articles/762084/