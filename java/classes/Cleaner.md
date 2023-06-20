### java.lang.ref.Cleaner


Cleaner нужен для выполнения очищающих действий.  
Cleaner регистрируется (метод `register`) для запуска, который произойдет если объект станет доступен
по фантомной ссылке (см. `references.md`). Cleaner использует PhantomReference и ReferenceQueue
чтобы проверить доступность объекта.

Каждый клинер работает независимо, выполняет очистительные действия, хендлит потоки и завершается после того
как перестает использоваться. Метод регистрации объекта возвращает `Cleanable`, у которого можно вызвать
метод `clean()`. Само очищающее действие задается объектом типа `Runnable` и выполняется (не больше одного раза),
когда объект становится доступным по фантомной ссылке, если он еще не был очищен явно.
Если объект не становится достижимым по фантомной ссылке, то клинер никогда не выполнится автоматически.

Выполнение клинера происходит в треде, с которым он связан. Все ошибки выброшенные при вызове клинера игнорируются.
Сам клинер не подвержен эксепшенам из runnable-действия, другие runnable-действия так же не подвержены ошибкам 
из текущего.
Тред будет жить, пока все клинеры не завершат свою работу и не будут очищены GC.

Поведение клиенров при вызове `System.exit` не гарантировано, и зависит от реализации.


Пример кода:
```java
public class CleaningExample implements AutoCloseable {
    // A cleaner, preferably one shared within a library
    private static final Cleaner cleaner = <cleaner>;

    static class State implements Runnable {
        State() {
            // initialize State needed for cleaning action
        }

        public void run() {
            // cleanup action accessing State, executed at most once
        }
    }

    private final State state;
    private final Cleaner.Cleanable cleanable;

    public CleaningExample() {
        this.state = new State();
        this.cleanable = cleaner.register(this, state);
    }

    public void close() {
        cleanable.clean();
    }
}
```

