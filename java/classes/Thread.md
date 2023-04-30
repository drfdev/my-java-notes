### Thread

Класс `java.lang.Thread`  
Класс представляет поток выполнения в программе.  
У потоков есть приоритеты, поток с высоким приоритетом имеет преимущество перед потоками с низким приоритетом.  
VM продолжает работать пока все потоки (не демоны) не завершатся

Запуск треда:
```java
Thread t = new Thread(() -> { ... });
t.start();
```

Список методов:
* static native Thread currentThread():
  Возвращает ссылку на текущий объект
* static native void yield():
  Подсказка планировщику потоков, что данный поток готов освободить свои ресурсы (использование процессора)
  Планировщик может проигнорировать данную инструкцию
* static native void sleep(long):
  Заставляет текущий поток заснуть на указанное количество милисекунд
* static void sleep(long, int):
  Заставляет текущий поток заснуть на милисекунд и наносекунд.
  Наносекунды задаются в промежутке 0-999999
* static void onSpinWait():
  Метод для реализации spin-loop
  Указывает на то, что сторона вызова в данный момент не может продолжать выполнение, пока не выполнятся одно или несколько событий
  Вызывая этот метод в конструкции spin-wait loop, вызывающий поток сигнализирует рантайму что он занят ожиданием
  Рантайм, в свою очередь, может выполнить какие-то действия для улучшения производительности
  Пример использования из документации
  ```java
  class EventHandler {
        volatile boolean eventNotificationNotReceived;
        void waitForEventAndHandleIt() {
            while ( eventNotificationNotReceived ) {
                java.lang.Thread.onSpinWait();
            }
            readAndProcessEvent();
        }
        void readAndProcessEvent() {
            // Read event from some source and process it
             . . .
        }
  }
  ```
* static boolean interrupted():
  Проверяет текущий поток на статус interrupted. Если поток был прерван, то при вызове этого метода данный статус
  будет снят. Второй вызов данного метода подряд всегда вернет false
* static Map<Thread, StackTraceElement[]> getAllStackTraces():
  Возвращает мапу со стектрейсами для всех живых тредов
  Смотри метод `getStackTrace`
* static void setDefaultUncaughtExceptionHandler(UncaughtExceptionHandler):
  Установить перехватчик сообщение по умолчанию.
  Перехватчик будет ловить ловить ошибки, которые завершают выполнения потоков (и не ловятся)
* static UncaughtExceptionHandler getDefaultUncaughtExceptionHandler():
  Возвращает перехватчик по умолчанию

---

Методы:
* synchronized void start():
  Запускает выполнение текущего потока
  JVM запускает метод `run()` для запуска потока
* void run():
  Запускает Runnable объект, которые задан в данном потоке
* void stop():
  Заставляет поток прекратить выполнение
  **Deprecated**, не стоит его использовать
* void interrupt():
  Прерывает поток
* boolean isInterrupted():
  Возвращает статус потока (прерван или нет)
  Не меняет статус потока
* native boolean isAlive():
  проверяет жив ли поток
  Поток является живым, если он был запущен и не был завершен (died)
* void suspend():
  Приостанавливает поток
  **Deprecated**, не стоит его использовать
* void resume():
  Запускает приостановленный поток
  **Deprecated**, не стоит его использовать
* void setPriority(int):
  Меняет приоритет потока
  Значение приоритета должно быть между  MIN_PRIORITY и MAX_PRIORITY (1 и 10)
  Приоритет по умолчанию 5
* int getPriority():
  Возвращает текущий приоритет потока
* synchronized void join(long):
  Ждет заданное количество миллисекунд пока поток умрет (закончится)
  Если передано значение 0 - ожидание будет бесконечным
* synchronized void join(long, int):
  Ждет заданное количество миллисекунд и наносекунд пока поток умрет
  Наносекунды могут быть в промежутке: 0-999999
  Если передано значение 0 - ожидание будет бесконечным
* void join():
  Ждет пока потом не умрет
  Аналогичен вызову `t.join(0L);`
* void setDaemon(boolean):
  Маркирует текущий поток как демон-поток
  JVM завершает работу, даже есть запущенные демон-потоки
* boolean isDaemon():
  Возвращает true если поток является демоном
* native boolean holdsLock(Object):
  Возвращает true, если текущий поток стоит на мониторе переданного объекта
* StackTraceElement[] getStackTrace():
  Возвращает список стектрейсов для текущег потока
* State getState():
  Возвращает стейт потока (см ниже описание стейтов)
* UncaughtExceptionHandler getUncaughtExceptionHandler():
  Возвращает обработчик uncaught ошибок
* void setUncaughtExceptionHandler(UncaughtExceptionHandler):
  Задать обработчик uncaught ошибок


---

Статус (Thread.State):
* NEW:
  Поток создан, но еще не запущен
* RUNNABLE:
  Состояние потока когда он был запущен.
  Поток в этом состоянии может выполняться или ожидать каких-нибудь ресурсов
* BLOCKED:
  Поток стоит на мониторе.
  Такой статус можно получить при попадании в синхронизованный блок в методе `Object.wait()`
* WAITING:
  Состояние ожидания
  Поток переходит в это состояние после вызова следующих методов:
  `Object.wait` with no timeout
  `Thread.join` with no timeout
  `LockSupport.park`
* TIMED_WAITING:
  Состояние ожидания с таймаутом
  Поток переходит в это состояние после вызова следующих методов:
  `Thread.sleep`
  `Object.wait` with timeout
  `Thread.join` with timeout
  `LockSupport.parkNanos`
  `LockSupport.parkUntil`
* TERMINATED:
  Состояние завершения потока, после того как поток закончил свое выполнение

