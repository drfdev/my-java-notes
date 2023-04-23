### StringTokenizer

Класс java.util.StringTokenizer  
Нужен для того, чтобы разбить строку на токены. Пример использования:
```java
StringTokenizer st = new StringTokenizer("this is a test");
while (st.hasMoreTokens()) {
    System.out.println(st.nextToken());
}
```

Результат:
```text
this
is
a
test
```

StringTokenizer является легаси классом, и существует только для поддержки обратной совместимости. 
Использовать этот класс в новом коде не рекомендуется. Вместо него лучше использовать метод `split` из класса String,
или классы из пакеджа `java.util.regex`

Пример на использование split:
```java
String[] result = "this is a test".split("\\s");
for (int x=0; x<result.length; x++)
    System.out.println(result[x]);
```

Результат вывода будет аналогичный:
```text
this
is
a
test
```

