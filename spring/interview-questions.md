### Spring interview questions

---

Spring scope:
* singleton (по умолчанию)
* prototype
* request
* session
* application
* websocket

**singleton**  
Единственный бин будет создан


**prototype**  
Каждый раз когда к данному бину идет обращение - создается новый инстанс  
Не singleton, для этого скоупа создается новый бин каждый раз когда идет обращение на создание этого бина
(например: инжект бина в другой бин или програмный вызов getBean() контейнера).
Данный скоуп следует использовать для бинов с состоянием, в то время как singleton стоит использовать для
stateless бинов.


**request**  
(Доступен только в web-aware Spring приложении)  
Бин создается на время жизни HTTP запроса


**session**  
(Доступен только в web-aware Spring приложении)  
Бин создается на время жизни HTTP сессии


**application**  
(Доступен только в web-aware Spring приложении)  
Бин создается на время жизни ServletContext


**websocket**  
(Доступен только в web-aware Spring приложении)  
Бин создается на время жизни WebSocket session


Info:
- https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes
- https://www.baeldung.com/spring-bean-scopes


---

Проблема циклических зависимостей

Будет выброшено BeanCurrentlyInCreationException.  
Варианты решения:
- не использовать циклические зависимости
- ленивая инициализация `@Lazy`
- внедрение зависимостей через setter-метод, а не через конструктор

Info:
- https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-dependency-resolution
- https://www.baeldung.com/circular-dependencies-in-spring


---

Bean Life Cycle

1) Container started
2) Bean instantiated
3) Dependencies injected
4) Custom init() method
5) Custom utility methods
6) custom destroy() method

Info:
- https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core
- https://habr.com/ru/articles/222579/
- https://www.geeksforgeeks.org/bean-life-cycle-in-java-spring/


---

`@Transactional`

Создает транзакцию, использует шаблон proxy


```java
public class MyServiceImpl {
  
  @Transactional
  public void method1() {
    //do something
    method2();
  }

  @Transactional (propagation=Propagation.REQUIRES_NEW)
  public void method2() {
    //do something
  }
}
```
Будет ли запускаться новая транзакция в методе 2?
Ответ: нет*
*фреимворк разработан таким образом чтобы проблем не возникало независимо какой тип проксирования используется



Info:
- https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html
- https://habr.com/ru/articles/532000/
- https://habr.com/ru/articles/347752/


---

