### PECS - Producer Extends Consumer Super

Принцип работает следующим образом:

Для списка `List<? extends Class1> list`:
* можно получить только объекты суперклассов
* нельзя ничего положить

Для списка `List<? super Class1> list`:
* Можно получить только объекты типа `Object`
* можно положить объекты подклассов класса `Class1`

---

Примеры:

* `List<? extends Number> values = new ArrayList<>();`

Корректно:
```java
Number v = values.get(0);
Object o = values.get(0);
```

Не компилируются:
```java
values.add(new Integer(1));
values.add(new Double(1));

Number n = new Integer(2);
values.add(n);

Serializable s = new Integer(3);
values.add(s);
```

* `List<? super Number> values = new ArrayList<>();`

Корректно:
```java
values.add(new Integer(1));
values.add(new Double(1));

Number n = new Integer(2);
values.add(n);

Object o = values.get(0);
```

Не компилируются:
```java
Serializable s = new Integer(3);
values.add(s);

Number v = values.get(0);
```