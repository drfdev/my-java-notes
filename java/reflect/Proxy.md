### Proxy

В java для проксирования используется два класса:  
`java.lang.reflect.Proxy`  
`java.lang.reflect.InvocationHandler`  

Создание прокси объекта:  

`Proxy.newProxyInstance(ClassLoader, Class<?>[], InvocationHandler)`  
loader - класс лоадер, для объявления прокси-класса  
interfaces – список интерфейсов, которые прокси класс должен реализовывать  
handler - обработчик для отправки вызовов метода  

Создание обработчика вызовов:  

```java
InvocationHandler handler = (proxy, method, args) -> {
    if(method.getName().equals("getName")){
        return ((String)method.invoke(user, args)).toUpperCase();
    }
    return method.invoke(user, args);
};
```

В обработчике всего один метод: `Object invoke(Object proxy, Method method, Object[] args)`  
proxy - прокси, на котором вызов метода происходит  
method - класс метода (из рефлексии), основано на интерфейсе, который реализует прокси  
args - массив аргументов, которые были переданы в вызов метода  

Для выполнения дефолтных методов, есть специальный метод `invokeDefault`, пример использования:
```java
// arrange
interface A {
    default T m(A a) {
        return t1; 
    }
}
interface B {
    default T m(A a) {
        return t2;
    }
}
interface C extends A {}

// proxy for A
// Код будет вызывать дефолтный метод в классе A::m
Object proxy = Proxy.newProxyInstance(loader, new Class<?>[] { A.class },
        (o, m, params) -> {
    if (m.isDefault()) {
        // if it's a default method, invoke it
        return InvocationHandler.invokeDefault(o, m, params);
    }
});

// Если прокси реализует оба интерфейса и A и B, то можно выбирать какой именно дефолтный метод мы будем вызывать
// далее вызывается дефолтный метод из B::m
Object proxy = Proxy.newProxyInstance(loader, new Class<?>[] { A.class, B.class },
        (o, m, params) -> {
    if (m.getName().equals("m")) {
        // invoke B::m instead of A::m
        Method bMethod = B.class.getMethod(m.getName(), m.getParameterTypes());
        return InvocationHandler.invokeDefault(o, bMethod, params);
    }
});

// Если проксируется C, то можно вызвать дефолтный метод из A
// далее прокси C и вызывается метод A::m
C c = (C) Proxy.newProxyInstance(loader, new Class<?>[] { C.class },
        (o, m, params) -> {
    if (m.getName().equals("m")) {
        // IllegalArgumentException thrown as {@code A::m} is not a method                  
        // inherited from its proxy interface C
        Method aMethod = A.class.getMethod(m.getName(), m.getParameterTypes());
        return InvocationHandler.invokeDefault(o, aMethod, params);
    }
});
c.m(...);
```

---

Пример кода:

```java
package proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class JdkProxyDemo {
    interface If {
        void originalMethod(String s);
    }
    static class Original implements If {
        public void originalMethod(String s) {
            System.out.println(s);
        }
    }
    static class Handler implements InvocationHandler {
        private final If original;
        public Handler(If original) {
            this.original = original;
        }
        public Object invoke(Object proxy, Method method, Object[] args)
                throws IllegalAccessException, IllegalArgumentException,
                InvocationTargetException {
            System.out.println("BEFORE");
            method.invoke(original, args);
            System.out.println("AFTER");
            return null;
        }
    }
    public static void main(String[] args){
        Original original = new Original();
        Handler handler = new Handler(original);
        If f = (If) Proxy.newProxyInstance(If.class.getClassLoader(),
                new Class[] { If.class },
                handler);
        f.originalMethod("Hallo");
    }
}
```

Результат работы программы:
```text
BEFORE
Hallo
AFTER
```

---

Info:
- https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Proxy.html
- https://habr.com/ru/articles/348906/
- https://www.baeldung.com/java-dynamic-proxies

