### java.util.Set

---

**java.util.HashSet**

---

**java.util.LinkedHashSet**

---

**java.util.TreeSet**

---

**java.util.SortedSet** (interface)

Все реализации должны поддерживать сортировку
Сортируются в натуральном порядке или через компоратор
Аналог **java.util.SortedMap**

Доступные методы:
* comparator:
  возвращает компаратор
* subSet(E fromElement, E toElement):
  Возвращает подмножество расположенное между двумя элементами
* headSet(E toElement):
  Возвращает головное подмножество до заданного элемента
* tailSet(E fromElement):
  Возвращает подмножество с конца до заданного элемента
* first() / last():
  Первый / последний элементы множества

---

**java.util.NavigableSet** (interface)

SortedSet расширенный навигационными методами
* E lower(E e):
  Возвращает наибольший элемент меньший чем заданный параметр, или null если такого элемента нет
* E floor(E e):
  Возвращает наибольший элемент меньший или равный чем заданный параметр, или null если такого элемента нет
* E ceiling(E e):
  Возвращает наименьший элемент больший или равный чем заданный параметр, или null если такого элемента нет
* E higher(E e):
  Возвращает наименьший элемент больший чем заданный параметр, или null если такого элемента нет
* E pollFirst():
  Возвращает и удаляет первый (наименьший) элемент или возвращает null, если Set пуст.
* E pollLast():
  Возвращает и удаляет последний (наибольший) элемент или возвращает null, если Set пуст.
* NavigableSet<E> descendingSet():
  Возвращает Set с обратным порядком элементом из текущего et-а.
  Set будет содержать зеркальный набор элементов. При изменении одного из Set-ов, во время итерирования другого - результат не определен

  Порядок должен быть аналогичный `Collections.reverseOrder(comparator())`
  Результат вызова `s.descendingSet().descendingSet()` должен быть эквивалентен `s`
* Iterator<E> descendingIterator():
  Возвращает итератор пробегающий элементы в обратном порядке
  Результат эквивалентен вызову `descendingSet().iterator()`
* NavigableSet<E> subSet(E, boolean, E, boolean):
  Возвращает представление части Set-а, элементы которого находятся в диапазоне от fromElement до toElement.
  Если fromElement и toElement равны, возвращается пустой Set, несмотря на то что fromInclusive и toInclusive оба имеют значения true.
  Возвращенный Set поддерживается текущим Set-ом, поэтому изменения в возвращенном Set-е отражаются в текущем Set-е, и наоборот.
  Возвращенный Set поддерживает все необязательные операции с Set-ом, которые поддерживает текущий Set.

  **fromElement** – нижняя конечная точка возвращенного Set-а
  **fromInclusive** – true если fromElement должна быть включена в результат
  **toElement** – высшая конечная точка возвращенного Set-а
  **toInclusive** – true если toElement должна быть включена в результат
* NavigableSet<E> headSet(E, boolean):
  Возвращает представление Set-а, содержащего элементы меньше или равны (если inclusive имеет значение true) элемента toElement

  **toElement** – высшая конечная точка возвращенного Set-а
  **inclusive** – true если toElement должна быть включена в результат
* NavigableSet<E> tailSet(E, boolean):
  Возвращает представление Set-а, содержащего элементы больше или равны (если inclusive имеет значение true) элемента fromElement

  **fromElement** – нижняя конечная точка возвращенного Set-а
  **inclusive** – true если fromElement должна быть включена в результат

---

**java.util.UnmodifiableSet** (in Collections)

Результат вызова `Collections.unmodifiableSet(Set<T>)`
Оборачивает существующий Set
Методы меняющие Set кидают UnsupportedOperationException, метод remove() итератора так же кидает UnsupportedOperationException

---

**java.util.SynchronizedSet** (in Collections)

Результат вызова `Collections.synchronizedSet(Set<T>)` или `Collections.synchronizedSet(Set<T>, Object)`
Оборачивает существующий Set, все методы синхронизуются по mutex-у
Mutex - может быть this или объект переданный в конструктор
Декорирует Set, примеры вызова:
```java
public boolean add(E e) {
    synchronized (mutex) {return c.add(e);}
}
public boolean isEmpty() {
    synchronized (mutex) {return c.isEmpty();}
}
```


---

**java.util.concurrent.ConcurrentSkipListSet**

---

**java.util.EmptySet** (in Collections)

Есть только один экземпляр данного класса, доступный по Collections.emptySet()
Функции ничего не делают, ничего не переопределено из AbstractSet
Хешкод всегда 0, пустые итераторы, защита от создание копии через сериализацию
