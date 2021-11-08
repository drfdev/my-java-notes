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
