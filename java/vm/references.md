### References

**Strong reference**

Обычная ссылка на объект. Когда мы пишет `T a = new T();` создается сильная ссылка a.
Такие объекты очищаются GC не раньше чем станут неиспользуемыми

Если мы сделаем такой вызов: `a = null;`, то ссылка станет доступной для текущего объекта,
и. если нет дополнительных ссылок, такая ссылка может быть очищена GC

---

**Weak reference**

Объявлена классом `WeakReference<T>`  
Если объект имеет только слабую ссылку, то такой объект может быть удален GC (нет сильной или soft ссылки)

Это означает, что если мы напишем такой код:
```java
// Сильная ссылка
Gfg g = new Gfg();
g.x();

// Создаем слабую ссылку 
WeakReference<Gfg> weakref = new WeakReference<Gfg>(g);

// Тут мы удаляем сильную ссылку на g
// g становится доступна для GC
// Но удаление данной ссылки будет тольк в том случае, если GC понадобится память
g = null; 

// Тут мы воостанавливаем сильную ссылку на g
g = weakref.get();

g.x();
```

---

**Soft reference**

Объявлена классом `SoftReference`  
Данная ссылка будет жить, пока JVM не понадобится освободить память.
Все мягкие ссылки будут очищены до наступления OutOfMemoryError

---

**Phantom reference**

Объявлена классом `PhantomReference`  
Такая ссылка помечает объет как готовый на удаление. Затем JVM кладет объект в очередь ссылок (объект помещается после
вызова метода finalize() на нем)  
Важно помнить что метод `get()` всегда возвращает null


---

Больше информации:
- https://www.geeksforgeeks.org/types-references-java/
- https://habr.com/ru/articles/169883/
