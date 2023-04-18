### Class Loaders

```java
public void printClassLoaders() throws ClassNotFoundException {

    System.out.println("Classloader of this class:"
        + PrintClassLoader.class.getClassLoader());

    System.out.println("Classloader of DriverManager:"
        + DriverManager.class.getClassLoader());

    System.out.println("Classloader of ArrayList:"
        + ArrayList.class.getClassLoader());
}
```

Ожидаемый результат:
```
Classloader of this class:jdk.internal.loader.ClassLoaders$AppClassLoader@73d16e93
Classloader of DriverManager:jdk.internal.loader.ClassLoaders$PlatformClassLoader@1fb700ee
Classloader of ArrayList:null
```

Есть три разных тип класс лоадеров:
- application
- extension
- bootstrap (показан как null)

Загрузка классов происходит от application, далее к extension, а потом в bootstrap. Если класс не найден ни в одном
из загрузчиков, то будет выкинуто исключение `java.lang.NoClassDefFoundError` или `java.lang.ClassNotFoundException`

Загрузчики вызываются по очереди, начиная от application, и если в нем ничего не найдено - идет обращение к родительскому
Классы загруженные разными загрузчиками видны их родительским загрузчикам: если класс А загружен application class loader,
а класс Б загружен extensions class loader, оба класс будут видны другим классам загруженным application class loader
Класс Б однако будет виден другим классам загруженным extensions class loader (а класс А - НЕТ!!)



---

**Bootstrap Class Loader**

Этот загрузчик классов несет ответственность за загрузку внутренних классов JDK,
обычно rt.jar и другие библиотеки из папки `$JAVA_HOME/jre/lib`  
Bootstrap Class Loader - является родительским для всех class loader-ов, является core-частью JVM и написан
на нативном коде (разные платформы могут иметь разные реализации)


---

**Extension Class Loader**

Это дочерний относительно bootstrap загрузчик классов  
Он ответственный за загрузку классов из JDK extensions directory (каталог расширений)  
Обычно это папка `$JAVA_HOME/lib/ext` или любые другие папки указанные в `java.ext.dirs` (системная настройка)


---

**System Class Loader**

Второе название _application class loader_  
Нужен для загрузки всех классов приложения в JVM. Загружает все классы найденные в classpath environment variable,
`-classpath` или `-cp` параметрах командной строки

Так же является дочерним по отношению к extensions class loader


---

Информация взята отсюда: https://www.baeldung.com/java-classloaders
