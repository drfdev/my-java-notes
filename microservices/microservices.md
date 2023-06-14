### Микросервисы


_Плюсы_  
* Гибкость
  Отдельные микросервисы могут быть написаны с использованием различных языков программирования и различных подходах
  Новую технологию можно внедрить быстро
* Надежность (отказоустойчивость)
  Микросервис, вышедщий из строя можно легко вывести из системы и быстро починить / откатить изменения
* Масштабирование
  Можно масштабировать только ту часть системы, которую необходимо
* Простота развертывания
  Можно внести изменения в один сервис и развернуть только его, без перезапуска все системы целиком
* Простота написания
  Каждый отдельный сервис содержит мало кода, поэтому гораздо проще его написать и разобраться в существующем


_Минусы_  
* Сложности мониторинга
  За сотней микросервисов труднее наблюдать чем за единым монолитом
* Перегрузка технологиями
  Использование разных технологий и подходом - это и плюс и минус. Когда технологий слишком много, когда каждая команда
  использует свой подход - разобраться в этом становится невероятно трудно
* Безопасность
  Авторизация между сервисами, передача данных по сети
* Тестирование
  Сложности с тестированием интеграционных цепочек
* Сложность развертывания
  Развернуть один микросервис просто, развернуть сотню микросервисов - проблема
* Время ожидания
  Дополнительные затраты на вызовы между сервисами (по сети)


---

Info:
- https://vc.ru/dev/719244-mikroservisy-preimushchestva-i-nedostatki
- https://habr.com/ru/companies/southbridge/articles/674600/
- https://simpleone.ru/blog/primenenie-mikroservisnoj-arhitektury-plyusy-minusy-podvodnye-kamni/#