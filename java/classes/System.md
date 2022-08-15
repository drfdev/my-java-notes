### Методы java.lang.System

###### arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
Метод копирования массивов
src - источник данных
dest - результат куда нужно сложить данные
srcPos - начальная позиция в массиве-источнике
destPos - начальная позиция в массиве-приемнике
length - количество элементов для копирования

###### Console console()
Возвращает уникальный объект Console ассоциированный с этой виртуальной машиной, если есть
Может вернуть null (возвращает null в unit-тестах)

###### long currentTimeMillis()
Возвращает текущее время в миллисекундах по часам операционной системы
Если операционная система оперирует не миллисекундами, а единицами времени большей размерности - ну что поделать

###### long nanoTime()
Возвращает время работы данной виртуальной машины джава в наносекундах
Метод должен использоваться для вычисления промежутков времени (истекающих)
Если время работы больше чем 292 года, то корректность работы не гарантируется
Значение данного метода осмысленно только для конкретной джава машины
Некорректно сравнивать его со значениями полученными в других джава машинах

Пример использования:
```java
long startTime = System.nanoTime();  // ... the code being measured ...
long elapsedNanos = System.nanoTime() - startTime;
```

###### void exit(int status)
Завершает текущую запущенную виртуальную машину
status - статус завершения

###### void gc()
Запускает сборку мусора
Вызов метода говорит джава машине, что сборку мусора нужно провести быстрее, чем она реально запланирована
После выхода из метода ВМ сделает попытку переназначить место из-под всех неиспользуемых объектов
Но никаких гарантий нет: не гарантируется удаление объектов, не гарантируется что будет освобождено место
или выполнение за конкретное время (после того как метод завершится)

###### Map<String,String> getenv()
###### String getenv(String name)
###### Logger getLogger(String name)
###### Logger getLogger(String name, ResourceBundle bundle)
###### Properties getProperties()
###### String getProperty(String key)
###### String getProperty(String key, String def)
###### void setProperties(Properties props)
###### String setProperty(String key, String value)
###### String clearProperty(String key)
###### SecurityManager getSecurityManager()
###### void setSecurityManager(SecurityManager sm)
###### int identityHashCode(Object x)
###### Channel inheritedChannel()
###### String lineSeparator()
###### void load(String filename)
###### void loadLibrary(String libname)
###### String mapLibraryName(String libname)
###### void runFinalization()
###### void setErr(PrintStream err)
###### void setIn(InputStream in)
###### void setOut(PrintStream out)

###### System.err
###### System.in
###### System.out

###### System.Logger

~~TODO~~
