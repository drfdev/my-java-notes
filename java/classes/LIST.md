### java.util.List

Списки: доступ по индексу, сортированная, элементы могут повторяться

---

**java.util.ArrayList**

Обертка над массивом: объекты лежат в Object[]
Конструкторы:
* ArrayList(): массив из 10 элементов
* ArrayList(int): создает массив заданной длины
* ArrayList(Collection): создает массив с данными, скопированными из предоставленной коллекции

Добавление:
* add:
  Добавление одного элемента в коллекцию
  Добавление в конец: добавляет по последнему индексу элемент, если в массиве нет места увеличивает в полтора раза
  Добавление в середину: если места не хватает -- увеличивает в полтора раза, затем копирует часть массива на 1 элемент (сдвигает) и вставляет по индексу
* addAll: 
  Добавление коллекции в массив, если она не пуста
  Добавление в конец: копирует данные в конец массива, если места не хватает -- сначала увеличивает размер массива
  Добавление в середину: если места не хватает увеличить массив, затем копируем конец массива, сдвигаем на нужную длину, затем копируем в освободившиеся ячейки
  Увеличение размера: максимальное из
    1) увеличить в полтора раза сумму текущего размера и добавляемой коллекции 
    2) сумма текущей длины и длины добавляемой коллекции

Удаление:
* remove(int):
  Удаление по индексу. Делает сдвиг на 1 элемент, затирая элемент по индексу
  Сдвиг через копирование массива
* remove(Object):
  Ищет элемент с начала массива, сравнение по equals()
  После нахождения происходит удаление по индексу
* removeAll:
  Удаляет все элементы которые содержатся в коллекции переданной в параметрах
  Алгоритм представляет собой перестановку элементов, содержащихся в переданной коллекции
* removeIf:
  Удаляет элементы из коллекции соответствующие предикату
  Алгоритм с перестановкой элементов в цикле
* removeRange:
  Удаляет все элементы в заданной промежутке
  Просто копирует элементы, затирая элементы в промежутке, назначая новую длину массива

Поиск по индексу:
* indexOf: 
  ищет элемент по equals с начала до конца списка
  Если передать ll - будет искать первый элемент равный null
* indexOfRange:
  Аналогично indexOf, только в заданном диапазоне индексов
* lastIndexOf:
  Поиск аналогичный indexOf, только не с начала, а с конца
* lastIndexOfRange:
  Поиск аналогичный indexOfRange, но с конца списка

Сравнение:
* equals:
  Сравнивает поэлементно через equals, может кинуть ConcurrentModificationException
* hashCode:
  hashCode = 31 * hashCode + element[i].hashCode
  По всем элементам

Другое:
* contains:
  return indexOf(o) >= 0;

---

**java.util.LinkedList**

Связанный список
Так же наследуется от Deque (двунаправленной очереди)
Структура:
Хранится две ссылки на первый и последний элемент: first и last
внутренний статический класс Node<E>: контейнер элемента с двумя ссылками next и prev (на следующий и предыдущий)

Конструкторы:
* без параметров
* LinkedList(Collection) - вызывает метод addAll()

Добавление:
**Внутренние методы**
linkFirst():
* создает новую ноду и перевешивает ссылки: новая нода становится первой 
linkLast():
* создает новую ноду и перевешивает ссылки: новая нода становится последней
unlinkFirst(Node):
* перевешивает ссылки удаляю первую ноду
unlinkLast(Node):
* перевешивает ссылки удаляя последнюю ноду
unlink(Node):
* перевешивает ссылки, выкидывая переданную ноду из списка
linkBefore(E, Node):
* создает новый узел с элементом E и вставляет его перед Node

**Доступные**
add(E):
* вызывает linkLast
add(int, E):
* Если индекс последний вызывает linkLast, для остальных linkBefore 
addAll():
* Бежит по коллекции и перевешивает ноды, добавляя новые
  Есть два варианта: (1) добавление в конец и (2) добавление по индексу
addFirst(E):
* вызывает linkFirst()
addLast(E):
* вызывает linkLast()

Удаление:
remove():
  Вызов removeFirst()
remove(int):
  Ищет ноду по индексу, затем выкидывает элемент из списка через перевешивание ссылок
remove(Object):
  Удаляет (разлинковывает) объект первый по equals(), если передан null - разлинковывает первый null
removeFirst():
  Берет первый элемент и удаляет (разлнковывает) его
removeFirstOccurrence(Object):
  Вызов remove(Object)
removeLast():
  Берет последний элемент и удаляет (разлинковывает) его
removeLastOccurrence(Object):
  Аналог remove(Object), но поиск объекта идет с конца списка. Поиск по equals() или на null

Поиск по индексу:
get(int):
  Ищет ноду по индексу, затем возвращает элемент лежащий в ней
getFirst():
  Возвращает элемент лежащий в первой ноде
getFirst():
  Возвращает элемент лежащий в последней ноде
contains(Object):
  return indexOf(o) >= 0;
element():
  Возвращает результат getFirst()
indexOf():
  Ищет элемент по equals (или по null) и возвращает индекс этого элемента

Сравнение:
equals:
  Создает два итератора, затем бежит по ним и поэлементно сравнивает через equals
hashCode:
  hashCode = 31 * hashCode + element[i].hashCode
  По всем элементам  

Другое:
offer(E):
  Вызов add(E)
offerFirst(E):
  Вызов addFirst(E)
offerLast(E):
  Вызов addLast(E)
peek():
  Возвращает первое значение
peekFirst():
  Возвращает первое значение, код аналогичен peek(), но почему-то в java 16 закопирован
peekLast():
  Возвращает последнее значение
poll():
  unlinkFirst()
pollFirst():
  unlinkFirst()
pollLast():
  unlinkLast()
push(E):
  addFirst()
pop():
  return removeFirst()



---

**java.util.Vector**
Обертка над массивом. Все методы синхонизованы
Содержит:
Object[] elementData
  массив элементов
int elementCount
  размер массива
int capacityIncrement
  количество на которое массив будет прирастать если размер массива достигает максимума.
  Если равен нулю, то увеличивает в 2 раза

Конструкторы:
Vector(int initialCapacity, int capacityIncrement)
  задает начальные значения
Vector(int initialCapacity)
  задает initialCapacity и capacityIncrement = 0
Vector()
  задает значения по умолчанию: initialCapacity = 10 и capacityIncrement = 0
Vector(Collection<? extends E> c)
  копирует массив

Добавление:
add(E e)
  **synchronized**
  Добавляет элемент, если элемент не вмещается массив увеличивается в размере
add(int index, E element)
  вызывает insertElementAt(element, index)
insertElementAt(E obj, int index)
  **synchronized**
  добавляет объект в позицию index, если размера не хватает массив увеличивается в размере
addElement(E obj)
  **synchronized**
  добавляет элемент в конец списка, если размера не хватает массив увеличивается в размере
addAll(Collection<? extends E> c)
  **synchronized**
  добавляет список элементов в конец, если размера не хватает массив увеличивается в размере
addAll(int index, Collection<? extends E> c)
  **synchronized**
  добавляет список в позицию index, если размера не хватает массив увеличивается в размере
  
Удаление:
bulkRemove(Predicate<? super E> filter)
remove(int index)
remove(Object o)
removeElement(Object obj)
removeElementAt(int index)
removeAll(Collection<?> c)
removeIf(Predicate<? super E> filter)
removeRange(int fromIndex, int toIndex)

Поиск по индексу:

Другое:



---

**java.util.UnmodifiableList** (in Collections)

Обертка над листом
Все методы модификации выкидывают UnsupportedOperationException, остальные просто дилегируют вызов делегату


---

**java.util.concurrent.CopyOnWriteArrayList**

---

**java.util.CheckedList**

Типобезопасная обертка над листом.
Есть две реализации: CheckedList и CheckedRandomAccessList. Выбираются по условию: `list instanceof RandomAccess`

Все методы для модификации списка вызывают метод typeCheck(Object)
Если не выполнено условие `o != null && !type.isInstance(o)` то выкидывает ошибка ClassCastException

Остальные методы представляют делегирование вызовов тех же методов списку



---

**java.util.CopiesList** (in Collections)

Неизменяемый список состоящий из n одинаковых элементов
Внутри содержит элемент E и длину списка n

Вызывается из метода:
```java
public static <T> List<T> nCopies(int n, T o)
```

Все методы редактирования кидают UnsupportedOperationException
Метод get() - возвращает элемент E
Метод size() - всегда возвращает n

contains():
Если n != 0, тогда просто сравнивает входящий элемент и E

indexOf():
Возвращает contains(o) ? 0 : -1

lastIndexOf():
Возвращает contains(o) ? n - 1 : -1

subList():
Возвращает CopiesList меньшего размера

hashCode():
hashCode of n repeating elements is 31^n + elementHash * Sum(31^k, k = 0..n-1)
this implementation completes in O(log(n)) steps taking advantage of
31^(2*n) = (31^n)^2 and Sum(31^k, k = 0..(2*n-1)) = Sum(31^k, k = 0..n-1) * (31^n + 1)

equals():
Поэлементное сравнение с E



---

**java.util.SingletonList** (in Collections)

Список с одним элементом. Обертка над одним элементом.
size() возвращает всегда 1
contains() возвращает true если элемент equals входящий параметр
get() если не 0 - выкидывает IndexOutOfBoundsException, если 0 - возвращает элемент
removeIf(), replaceAll() кидают UnsupportedOperationException
hashCode() стандартный `return 31 + Objects.hashCode(element);`

---

**java.util.EmptyList** (in Collections)

Пустой массив. Константа в Collections, которая возвращается при вызове Collections.emptyList()
size() возвращает всегда 0
isEmpty() всегда возвращает true
contains() всегда возвращает false
containsAll() возвращает true, если входящий массив пустой
equals() возвращает true, если список пустой
hashCode() всегда возвращает 1

Переопределен readResolve() чтобы нельзя было создать два таких массива