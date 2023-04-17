### Lock Coarsening

Данный код:
```java
void m() {
    synchronized (this) {
        x = 1;
    }
    synchronized (this) {
        y = 1;
    }
}
```

может быть переписан самой jmm в такой вид:
```java
void m() {
    synchronized (this) {
        y = 1;
        x = 1;
    }
}
```

Объяснение такое:  
Если мы читали данные вещи без синхронизации, то мы их читали через гонку и плевать на порядок
Если мы записи читали под синхронизацией, то не обе записи будут сразу (или ни одна из них не будет), и в этом
случае все равно на порядок

---

### IRIW test (Independent Read Independent Write)

Дано:
```java
volatile int x, y;
```

| Thread 1 | Thread 2 | Thread 3                      | Thread 4                      |
|----------|----------|-------------------------------|-------------------------------|
| x = 1    | y = 1    | int r1 = y; <br/> int r2 = x; | int r3 = y; <br/> int r4 = y; |

(r1, r2, r3, r4) = (0, 1, 0, 1) запрещено JMM  
Результат может соответствовать только исполнению, где все synchronization actions образуют
линейный порядок, согласованный с программным, и где чтения видят предыдущие записи в
этом порядке.  
`volatile` - последовательно согласованы  
Если `x = 1` и `y = 1` случилось, то оно случилось в каком-то порядке

---

### Program order

Связывает действия в одном потоке  
Свойства:
- линейный порядок в каждом из потоков (ничего не говорится о порядке исполнения, только порядок в исходном коде)
- PO consistency: действия в одном потоке согласованы с действиями в программе
- нужны для того, чтобы пронести в модель информацию об исходной программе, поэтому нарушить PO нельзя


---

### Synchronization Order

Связывает специальные действия (synchronized actions)  
Свойства и инварианты:
- линейный порядок (ничего не говорит о порядке исполнения, только о видимом порядке эффектов)
- SO consistency: чтения должны видеть последнюю запись и только ее
- SO-PO consistency: порядки в SO и PO согласованы


---

### Synchronizes-with Order

SW - порядок SO  
Свойства и инварианты:
- SW - частичный порядок, установленный только между парами операций которые видят друг друга
- Важен для протаскивания между потоками


---

### Happens-Before order

HB - транзитивное замыкание объединения PO на SW  
Свойства и инварианты:
- частичный порядок, не все связано HB
- HB consistency: чтения видят (1) либо последовательную запись в HB, (2) либо любую другую запись не связанную HB [читает через гонку]


---

### Double Check Locking

```java
final Supplier<T> supp;
volatile T val;

public T get() {
    if (val == null) {
        synchronized (this) {
            if (val == null){
                val = supp.get();
            }
        }
    }
    return val;
}
```

Вариант 2:
```java
volatile Supplier<T> supp;
T val;

public T get() {
    if (supp != EMPTY) {
        synchronized (this) {
            if (val == null){
                val = supp.get();
                supp = EMPTY;
            }
        }
    }
    return val;
}
```


---

### Безопасная Публикация

Все изменения в потоках идут через пары release - acquire  
Все действия произведенные в первом потоке до release будут видны во втором потоке после acquire  

Гарантируются языком:
1) volatile read --> volatile write (HB) [если read видит write]
2) monitor exit --> monitor enter (HB) [если в таком порядке]
3) Thread.start --> первое действие в потоке (HB) 
4) последнее действие в потоке --> Thread.join (HB)
5) запись по умолчанию --> первое действие в потоке (HB)

В java-библиотеке:
1) Executor.submit() --> Callable.call() (HB)
2) ConcurrentHashMap.put() --> ConcurrentHashMap.get() (HB)
3) Semaphore.release() --> Semaphore.acquire() (HB)
4) Future.set() --> Future.get() (HB)
5) ...

По факту требуется сделать правильный release и acquire между потоками  
Чтобы все работало корректно


---

### Инициализация final

Для final полей если специальный freeze-action, который гарантирует HB чтение  
Т.е. если в объекте есть final-поля, то при чтении через гонку эти поля будут доступны через HB

Но важно помнить, что final спасает от гонки только на новом объекте, если он инициализируется через
гонку, то отмыться уже нельзя (значения полей приходят через гонку)


---

### Безопасная гонка

Пример небезопасной гонки:
```java
public class String {
    int hash;
    public int hashCode() {
        if (hash == 0 && value.lenght > 0) {
            hash == ...;
        }
        return hash;
    }
}
```

Пример безопасной гонки:
```java
public class AbstractMap<K, V> {
    transient Set<K> keySet; // non-volatile
    public Set<K> ketSet() {
        Set<K> ks = keySet; // RULE 1: single read
        if (ks == null) {
            ks = new KeySet(); // RULE 2: safety initialized
            keySet = ks;
        }
        return ks;
    }
}
```

Корректная реализация hashCode в java17 (упрощенная):
```java
public class String {
    public int hashCode() {
        int h = hash;
        if (h == 0) {
            h = ...;
            hash = h;
        }
        return h;
    }
}
```


---

### Выводы

1) Главные правила: безопасная публикация + безопасная инициализация
2) безопасная гонка: (1) одно чтение, (2) безопасно инициализирован

