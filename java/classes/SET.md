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
* E floor(E e):
* E ceiling(E e):
* E higher(E e):
* E pollFirst():
* E pollLast():
* NavigableSet<E> descendingSet():
* Iterator<E> descendingIterator():
* NavigableSet<E> subSet(E, boolean, E, boolean):
* NavigableSet<E> headSet(E, boolean):
* NavigableSet<E> tailSet(E, boolean):

---

**java.util.UnmodifiableSet** (in Collections)

---

**java.util.SynchronizedSet** (in Collections)

---

**java.util.concurrent.ConcurrentSkipListSet**

---

**java.util.EmptySet** (in Collections)

Есть только один экземпляр данного класса, доступный по Collections.emptySet()
Функции ничего не делают, ничего не переопределено из AbstractSet
Хешкод всегда 0, пустые итераторы, защита от создание копии через сериализацию
