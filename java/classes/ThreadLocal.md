### ThreadLocal

Класс предоставляет доступ для переменной локальной для этого потока  
Работает следующим образом:  
К каждому потоку привязана `ThreadLocalMap`, с entry `WeakReference<ThreadLocal<?>>`  
Когда мы вызываем метод get() для текущего потока берется данная мапа, из нее поднимается entry по текущему ThreadLocal
объекту (вызов getEntry(this)) и уже из этого объекта возвращается значение

в виде кода это выглядит так:
```java
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }
```

Intellij Idea постоянно напоминает, что нужно очищать ThreadLocal после использования во избежании утечек памяти
