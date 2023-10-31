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


## Безымянные классы (Unnamed Classes and Instance Main Methods) (java 21+)

~~TODO~~

```java
class HelloWorld {
    void main() {
        System.out.println("Hello, World!");
    }
}
```



---

Ссылки:
- JEP-443, Unnamed Patterns and Variables: https://openjdk.org/jeps/443
- JEP-445, Unnamed Classes and Instance Main Methods: https://openjdk.org/jeps/445
- https://habr.com/ru/articles/762084/