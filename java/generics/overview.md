### Generics

**Naming**
* E: Element (used extensively by the Java Collections Framework)
* K: Key
* N: Number
* T: Type
* V: Value
* S,U,V etc.: 2nd, 3rd, 4th types

---

**wildcard**

wildcard: `?`
upper bound: `extends`
lower bound: `super`

Объяснение:

`printAll(MyList<? extends Serializable>)`
More accurately, it means a call to printAll will compile only if it is passed a MyList
with some generic type that is or implements Serializable.
In this case it would accept a MyList<Serializable>, MyList<Integer>, etc.


`printAll(MyList<? super MyClass>)`
A wildcard bounded with super is a lower bound.
So we could say a call to printAll will compile only if it is passed a MyList with some generic type
that is MyClass or some super-type of MyClass.
So in this case it would accept MyList<MyClass>, e.g. MyList<MyParentClass>, or MyList<Object>.


`? extends T`: an unknown type which is a subtype of `T`
`? super T`: an unknown type which is a super type of `T`


Можно посмотреть еще тут: https://docs.oracle.com/javase/tutorial/extra/generics/morefun.html

---

**Multiple Bounds**

Пример использования:`<T extends Number & Comparable>`


---

**Примеры**

Дано:
```java
public static interface A {
}

public static interface B {
}

public static interface C {
}

public static class A_B implements A, B {
}

public static class A_C implements A, C {
}

public static class B_C implements B, C {
}

public static class A_B_C implements A, B, C {
}

public static class A_B_i_C extends A_B implements C {
}

public static class A_C_i_B extends A_C implements B {
}

public static class B_C_i_A extends B_C implements A {
}
```

### ---

Для такого:
`List<A> values = new ArrayList<>();`

Валидные варианты:
```java
values.add(new A() {});
values.add(new A_B());
values.add(new A_C());
values.add(new A_B_C());
values.add(new A_B_i_C());
values.add(new A_C_i_B());
values.add(new B_C_i_A());
```

### ---

Для такого:
`List<? extends A> values = new ArrayList<>();`

Валидных вариантов не будет, любой вызов `add` будет выдавать ошибку компиляции

### ---

Для такого:
`List<? super A> values = new ArrayList<>();`

Валидные варианты:
```java
values.add(new A() {});
values.add(new A_B());
values.add(new A_C());
values.add(new A_B_C());
values.add(new A_B_i_C());
values.add(new A_C_i_B());
values.add(new B_C_i_A());
```

### ---

Для методов:
```java
public static <T> void method(T value) {
    // nothing
}
```

Любой вариант подходит

### ---

Если метод сделать таким:
```java
public static <T extends A> void method(T value) {
        // nothing
}
```

Подходят варианты:
```java
method(new A() {});
method(new A_B());
method(new A_C());
method(new A_B_C());
method(new A_B_i_C());
method(new A_C_i_B());
method(new B_C_i_A());
```

### ---

Вариант `public static <T super A> void method(T value)` невозможен, будет ошибка компиляции

### ---

Для `Class` можно сделать два варианта:

`public static <T extends A> void method(Class<T> value)`
или
`public static <T extends A> void method(Class<? extends T> value)`

Валидные варианты:
```java
method(new A() {}.getClass());
method(A_B.class);
method(A_C.class);
method(A_B_C.class);
method(A_B_i_C.class);
method(A_C_i_B.class);
method(B_C_i_A.class);
```

### ---

Метод `get(int)` из `List`:

1) Вариант: `List<A> values = new ArrayList<>();`
   Рабочий вариант: `A a = values.get(0);`

2) Вариант: `List<? extends A> values = new ArrayList<>();`
   Рабочий вариант: `A a = values.get(0);`

3) Вариант: `List<? super A> values = new ArrayList<>();`
   Не компилируется: `A a = values.get(0);`
   А вот так работает: `Object a = values.get(0);`

4) Вариант: `List<? super A> values = new ArrayList<>();`

### ---

Комбинированный метод: `public static <T extends A & B> void meth(T val)`

Корректные варианты:
```java
method(new A_B());
method(new A_B_C());
method(new A_B_i_C());
method(new A_C_i_B());
method(new B_C_i_A());
```

### ---

Второй вариант комбинированного метода: `public static <T extends A_B & B> void method(T val)`

Корректные варианты:
```java
method(new A_B());
method(new A_B_i_C());
```
