### java.util.Set

---

**java.util.HashSet**

Реализация hash table. Обертка над HashMap.
Класс разрешает null-значение, не синхронизован.

Поля:
`HashMap<E,Object> map` - реализация основной логики хеш таблицы
`Object PRESENT = new Object();` - dummy-объект для работы с мапой

Доступные методы:
* iterator():
  Возвращает результат `map.keySet().iterator()`
* size():
  Возвращает результат `map.size()`
* isEmpty():
  Возвращает результат `map.isEmpty()`
* contains(Object o):
  Возвращает результат `map.containsKey(o)`
* add(E e):
  Возвращает результат `map.put(e, PRESENT)==null`
* remove(Object o):
  Возвращает результат `map.remove(o)==PRESENT`
* clear():
  Вызывает результат `map.clear()`

---

**java.util.LinkedHashSet**

Реализация HashSet через LinkedHashMap
Внутренний состав аналогичен `java.util.HashSet`, где вместо `HashMap<E,Object> map` используется `LinkedHashMap<E,Object> map`

Переопределен метод 
* public Spliterator<E> spliterator(): возвращает сплитератор с характеристиками DISTINCT и ORDERED (все элементы различны и отсортированы)

---

**java.util.TreeSet**

Множество базируется на `TreeMap`, наследует интерфейс `NavigableSet`
Сортировка элементов идет или в _натуральном порядке_, или через компаратор заданный в конструкторе
Не синхронизована

Поля:
* NavigableMap<E,Object> m:
  внутренняя мапа для работы (декорируемая)
* Object PRESENT = new Object():
  Dummy value to associate with an Object in the backing Map
  используется как значение для мапы `m`

Конструкторы:
* TreeSet():
  создает новое множество, в m кладется реализация `new TreeMap<>()`
* TreeSet(Comparator):
  реализация с заданным компаратором, в m кладется реализация `new TreeMap<>(comparator)`
* TreeSet(Collection<? extends E>):
  вызывает пустой конструктор, затем метод `addAll()`
  в m кладется реализация `new TreeMap<>()`
* TreeSet(SortedSet<E> s):
  вызывается конструктор с компаратором (компаратор берется из множества), затем метод `addAll()`

Методы:
* Iterator<E> iterator():
  возвращает `m.navigableKeySet().iterator()`, итератор пробегающий по множеству в прямом порядке
* Iterator<E> descendingIterator():
  возвращает `m.descendingKeySet().iterator()`, итератор пробегающий по множеству в обратном порядке
* NavigableSet<E> descendingSet():
  возвращает `new TreeSet<>(m.descendingMap())`
  Создает новый объект!
* size():
  возвращает `m.size()`, просто количество элементов в мапе/множестве
* isEmpty():
  возвращает `m.isEmpty()`, пустая мапа/множестве или нет
* contains(Object o):
  возвращает `m.containsKey(o)`, если элемент в мапе/множестве или нет
* add(E e):
  добавляет элемент в множество, кладет в карту `m.put(e, PRESENT)` - ключ наш элемент, значение dummy-объект
* remove(Object o):
  удаляет элемент из множества/мапы
* addAll(Collection<? extends E>):
  кладет все значения коллекции в мапу/множество
  есть оптимизация для `SortedSet` и пустой мапы
* NavigableSet<E> subSet(E, boolean, E, boolean):
  создает новый объект `TreeSet`, содержащий подмножество начинающееся с первого элемента, до последнего элемента
  boolean переменные определяют, будет ли включаться начало и конец промежутка или нет
  вызывает `new TreeSet<>(m.subMap(fromElement, fromInclusive, toElement,   toInclusive))`
* NavigableSet<E> headSet(E, boolean):
  создает новый объект `TreeSet`, содержит подмножество с головы до указанного элемента
  вызывает `new TreeSet<>(m.headMap(toElement, inclusive));`
* NavigableSet<E> tailSet(E, boolean):
  создает новый объект `TreeSet`, содержит подмножество начинающееся с указанного элемент до конца множества
  вызывает `new TreeSet<>(m.tailMap(fromElement, inclusive))`
* SortedSet<E> subSet(E, E):
  вызывает `subSet(fromElement, true, toElement, false)`
* SortedSet<E> headSet(E):
  вызывает `headSet(toElement, false)`
* SortedSet<E> tailSet(E):
  вызывает `tailSet(fromElement, true)`
* Comparator<? super E> comparator():
  возвращает компаратор используемый для сортировки в мапе/множестве
  возвращает `m.comparator()`
* E first():
  возвращает первый элемент, вызывает `m.firstKey()`
* E last():
  возвращает последний элемент, вызывает `m.lastKey()`
* E lower(E e):
  возвращает наибольший элемент, строго меньше чем заданный элемент
  вызывает `m.lowerKey(e)`
* E floor(E e):
  возвращает наибольший элемент, меньше или равный чем заданный элемент
  вызывает `m.floorKey(e)`
* E ceiling(E e):
  возвращает наименьший элемент, больший или равный чем заданный элемент
  вызывает `m.ceilingKey(e)`
* E higher(E e):
  возвращает наименьший элемент, строго больший чем заданный элемент
  вызывает `m.higherKey(e)`
* E pollFirst():
  возвращает и удаляет первый элемент
  вызывается `m.pollFirstEntry()`
* E pollLast():
  возвращает и удаляет последний элемент
  вызывается `m.pollLastEntry()`


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

Конкурентная реализация NavigableSet, базируется на ConcurrentSkipListMap
Предоставляет lon(n) время для операции contains, добавления,удаления и вариаций этих операций
Метод size работает НЕ константное время
Балковые операции  addAll, removeIf или forEach НЕ ГАРАНТИРУЮТ атомарность

Конструкторы:
* ConcurrentSkipListSet():
  создает новый пустой ConcurrentSkipListMap
* ConcurrentSkipListSet(Comparator):
  создает новый пустой ConcurrentSkipListMap с заданным компаратором
* ConcurrentSkipListSet(Collection):
  создает новый ConcurrentSkipListMap и вызывает addAll()
* ConcurrentSkipListSet(SortedSet):
  создает новый ConcurrentSkipListMap, с компаратором из SortedSet, затем вызывает addAll
* ConcurrentSkipListSet(ConcurrentNavigableMap):
  просто `this.m = m`

Хранит внутри себя ConcurrentNavigableMap<E,Object>
Вместо Object всегда добавляет Boolean.TRUE

Все методы, просто декорирование соответствующих методов из ConcurrentNavigableMap
См. TreeSet, тут примерно то же самое

---

**java.util.EmptySet** (in Collections)

Есть только один экземпляр данного класса, доступный по Collections.emptySet()
Функции ничего не делают, ничего не переопределено из AbstractSet
Хешкод всегда 0, пустые итераторы, защита от создание копии через сериализацию
