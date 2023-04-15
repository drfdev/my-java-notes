### java.util.concurrent

##### Atomics:  

Атомики представляют собой классы, которые обновляются атомарно.  
Это значит что в один момент времени обновить значение поля внутри атомика, может только один поток.
Для этого используется CAS (compare and swap) инструкция процессора, что является не блокирующей атомарной операцией  

CAS принимает 3 параметра:
1) место в памяти над которым нужно провести операцию (M)
2) существующее значение переменной (A)
3) новое значение которую нужно установить (B)

CAS атомарно записывает значение B в M, если значение в M равно A. В противном случае ничего не делает.
В обоих случаях возвращается текущее значение из M.
Когда несколько тредов пытаются обновить одно значение, только один выигрывает и обновляет, но остальные не останавливаются.
Проигравшие треды просто получают уведомление, что они не смогли обновить.
В этом случае треды просто повторяют операцию, пока она не завершится успехом.

Основные методы:
* get():
  получаем текущее значение
* incrementAndGet():
  увеличиваем значение на единицу, а затем получаем результат
* set():
  устанавливаем новое значение
* compareAndSet(expected, newValue)
  обновляет значение переменной (2 параметр), если оно равно ожидаемому (1 параметр)

Так же есть дополнительные методы доступа к данным, различаются они по типам (см. VarHandle.AccessMode)
Есть пять типов доступа:
1) Доступ на чтение (Read Access):
  Доступ на чтение, для получения значения переменной (с эффектами упорядочивания памяти)
2) Доступ на запись (Write Access):
  Доступ на запись нового значения в переменную (с эффектами упорядочивания памяти)
3) Atomic Update Access:
  Предоставление доступа для атомарной записи (аналогично compareAndSet())
  Тут нужно задать 2 значение: новое и старой, запись пройдет если текущее значение равно старому,
  в противном случае никаких изменений не будет
4) Numeric Atomic Update Access:
  Предоставляет числовые операции аналогичные getAndAdd(), с эффектами упорядочивания памяти
5) Bitwise Atomic Update Access:
  Этот метод записи предоставляет атомарные побитовые операции, с эффектами упорядочивания памяти

Эффекты упорядочивания памяти (Memory Ordering Effects):
* Plain:
  чтение и запись гарантируется побитово атомарной для примитивных типов или ссылок короче 32 бит.
  так же нет ограничений на порядок по отношению к остальным
* Opaque:
  чтение и запись побитово атамарные и в когерентном порядке по отношению в одной переменной
* Acquire / Release:
  подчиняются правилам как Opaque
  Acquire-чтение будет идти всегда после Release-записи
* Volatile
  операции полностью упорядоченные по отношению друг к другу


Подробнее тут: https://www.baeldung.com/java-variable-handles

---

**AtomicBoolean**

Булевое значение, которое меняется атомарно  

Методы:
* get():
  возвращает текущее значение (как volatile)
* set(boolean):
  устанавливает текущее значение (как volatile)
* compareAndSet(boolean, boolean):
  обновляет значение переменной на новое (2 параметр) если оно равно текущему (1 параметр)
  возвращает true если успешно обновил
* weakCompareAndSet(boolean, boolean):
  Deprecated с 9 версии
* weakCompareAndSetPlain(boolean, boolean):
  обновляет значение переменной на новое (2 параметр) если оно равно текущему (1 параметр)
  работает как weakPlain
* lazySet(boolean):
  обновляет значение (как release)
* getAndSet(boolean):
  возвращает текущее значение, а затем обновляет его на новое
* compareAndExchange(boolean, boolean):
  атомарно обновляет значение на новое (2 параметр) если оно равно ожидаемому (1 параметр) (как volatile)
* compareAndExchangeAcquire(boolean, boolean):
  аналогичное обновление, но использует обычный set() для установки значения, и acquire для чтения
* compareAndExchangeRelease(boolean, boolean):
  аналогично, но использует setRelease() и обычный get()
* weakCompareAndSetVolatile(boolean, boolean):
  аналогично методу compareAndSet, но может упасть на memory contention (как volatile)
* weakCompareAndSetAcquire(boolean, boolean):
  аналогично методу compareAndSet, но может упасть на memory contention (обычный set() и getAcquire())
* weakCompareAndSetRelease(boolean, boolean):
  аналогично методу compareAndSet, но может упасть на memory contention (setRelease() и обычный get())
* getPlain():
  возвращает значение (как plain, т.е. без учета volatile)
* setPlain(boolean):
  запись значения (как plain)
* getOpaque():
  получение значение (как Opaque)
* setOpaque(boolean):
  установка значения (как Opaque)
* getAcquire():
  получение значения (как Acquire)
* setRelease(boolean):
  запись значения (как Release)

---

**AtomicInteger**

Значение int, изменяются атомарно

Методы:
* get():
  Получить значение (как volatile)
  аналогичные метод: getPlain(), getOpaque(), getAcquire() 
* set(int):
  задать значение (как volatile)
  аналогичные метод: setPlain(int), setOpaque(int), setRelease(int)
* lazySet(int):
  задать значение как release
* getAndSet(int):
  атомарно задает новое значение, и возвращает старое
* compareAndSet(expectedValue, newValue):
  атомарно обновляет значение на новое newValue, если оно равно expectedValue
* weakCompareAndSetPlain(expectedValue, newValue):
  работает аналогично compareAndSet, но может упасть на memory contention (как plain)
  аналогичные методы: weakCompareAndSetVolatile(int, int), weakCompareAndSetAcquire(int, int), weakCompareAndSetRelease(int, int)
* getAndIncrement():
  делает атомарный инкремент текущего значения, и возвращает старое
  эквивалент getAndAdd(1)
* getAndDecrement():
  делает атомарный декремент текущего значения, и возвращает старое
  эквивалент getAndAdd(-1)
* getAndAdd(int):
  атомарно добавляет дельту к текущему значению, и возвращает предыдущее
* incrementAndGet():
  атомарный инкремент и возвращает измененное значение
  эквивалент addAndGet(1)
* decrementAndGet():
  атомарный декремент и возвращает измененное значение
  эквивалент addAndGet(-1)
* addAndGet(int):
  атомарно добавляет заданное значение к текущему, и возвращает измененное значение
* getAndUpdate(IntUnaryOperator):
  атомарно обновляет значение используя заданную функцию, возвращает предыдущее значение
* updateAndGet(IntUnaryOperator):
  атомарно обновляет значение используя заданную функцию и возвращает измененное значение
* getAndAccumulate(int, IntBinaryOperator):
  аналогично getAndUpdate(), но с бинарным оператором, где первый параметр - это второй параметр функции
* accumulateAndGet(int, IntBinaryOperator):
  аналогично updateAndGet(), но с бинарным оператором, где первый параметр - это второй параметр функции
* intValue():
  возвращает текущее значение (тип int, как volatile)
  похожие методы: longValue(), floatValue(), doubleValue()
* compareAndExchange(expectedValue, newValue):
  атомарно устанавливает значение newValue, если оно равно expectedValue
  возвращает проверяемое значение, если успех - то вернется значение равное expectedValue
  аналогичные методы: compareAndExchangeAcquire(int, int), compareAndExchangeRelease(int, int)

---

**AtomicLong**

Аналогичный _AtomicInteger_ класс, методы практически совпадают

---

**AtomicReference**

---

**AtomicIntegerArray**

---

**AtomicLongArray**

---

**AtomicReferenceArray**

---

**AtomicMarkableReference**

---

**AtomicStampedReference**

---

**DoubleAccumulator**

---

**DoubleAdder**

---

**LongAccumulator**

---

**LongAdder**

