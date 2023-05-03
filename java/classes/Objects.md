### java.util.Objects

###### boolean equals(Object a, Object b)

Возвращает true, если объекты одинаковые  
Реализация такая: `return (a == b) || (a != null && a.equals(b));`


###### boolean deepEquals(Object a, Object b)

Возвращает true, если объекты одинаковые  
Сравнивает через вызов `Arrays.deepEquals(a, b)`


###### int hashCode(Object o)

Возвращает hash code объекта, или 0 если передан null


###### int hash(Object... values)

Возвращает результат выполнения `Arrays.hashCode(values);`


###### String toString(Object o)

Возвращает toString объекта  
Реализация такая: `String.valueOf(o)`


###### String toString(Object o, String nullDefault)

Возвращает toString объекта, или nullDefault если объект null


###### <T> int compare(T a, T b, Comparator<? super T> c)

Сравнивает объекты через заданный компоратор  
Результат аналогичен вызову `c.compare(a, b)`


###### <T> T requireNonNull(T obj)

Проверяет объект на null, если объект null - будет брошен NullPointerException


###### <T> T requireNonNull(T obj, String message)

Аналогичен предыдущему requireNonNull  
Строка message будет передаваться в ошибку NullPointer


###### boolean isNull(Object obj)

Возвращает true, если объект null  
Метод нужен для стримов, использование `Objects::isNull`


###### boolean nonNull(Object obj)

Возвращает true, если объект не null
Метод нужен для стримов, использование `Objects::nonNull`


###### <T> T requireNonNullElse(T obj, T defaultObj)

Возвращает первый аргумент, если он null возвращается второй аргумент  
Если второй аргумент тоже null - будет выброшен NullPointerException


###### <T> T requireNonNullElseGet(T obj, Supplier<? extends T> supplier)

Возвращает первый аргумент, если он null вызывается supplier и возвращается результат его выполнения  
Если результат выполнения null - будет выброшен NullPointerException


###### <T> T requireNonNull(T obj, Supplier<String> messageSupplier)

Если первый аргумент null, тогда будет прошел NullPointerException с сообщением полученных из messageSupplier

