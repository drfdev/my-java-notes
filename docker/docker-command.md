### Docker CLI

`docker –version`  

Команда возвращает текущую версию докера


`docker pull <image name>`  

Команда пулит образ указанный в <image name> из репозитория докера (hub.docker.com по умолчанию)


`docker run -it -d <image name>`  

Команда используется для создания контейнера из образа


`docker ps`  
`docker ps -a`  

Показывается список процессов докера (запущенных контейнеров)  
Параметр -a нужен для отображения все контейнеров (запущенных или существующих)


`docker exec -it <container id> bash`  

Команда позволяет получить доступ к запущенному контейнеру по его <container id>


`docker stop <container id>`  

Останавливает контейнер по его <container id>  
Остановка и удаление всех контейнеров: `docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)`


`docker kill <container id>`  

Убивает запущенный контейнер по его <container id>.  
Останавливает контейнер мгновенно, разница между `docker stop` и `docker kill` в том, что stop выполняет
graceful shutdown, а kill просто завершает контейнер


`docker commit <conatainer id> <username/imagename>`  

Создает новый образ редактируемого контейнера в локальной системе


`docker push <username/image name>`  

Пушит образ в докер-репозиторий


`docker login`  
`docker login localhost:8080`  

Команда используется для логина в докер-репозиторий  
Можно логиниться на локальный репозиторий


`docker logout`  
`docker logout localhost:8080`  

Позволяет отключиться от докер-репозитория


`docker images`  

Показывает все локальные образы


`docker search <name>`  
`docker search <name> --filter stars=3 --no-trunc busybox`  

Поиск образа в докер-репозитории


`docker rm <container id>`  
`docker rm -v <container id>`  

Удаляет остановленный докер-контейнер  
Параметр -v удаляет том (volume) данного образа  
Удаление всех контейнеров со статусом exited: `docker rm $(docker ps -a -f status=exited -q)`


`docker container prune`

Удаление всех остановленных контейнеров  
Еще вариант:
```
docker rm `docker ps -a -q`
```
Удаление контейнеров, остановленных более суток назад: `docker container prune --filter "until=24h"`


`docker image prune`  

Удаление неиспользуемых образов  
Еще вариант: `docker rmi $(docker images -f dangling=true -q)`  
Удаление неиспользуемых (dangling) образов даже с тегами: `docker image prune -a`


`docker rmi <image-id>`  

Команда удаляет образ из локального хранилища  
Удаление всех образов без тегов: `docker rmi -f $(docker images | grep "^<none>" | awk "{print $3}")`


`docker volume prune`  

Удаление неиспользуемых (dangling) томов  
Еще вариант: `docker volume rm $(docker volume ls -f dangling=true -q)`  
Удаление неиспользуемых (dangling) томов по фильтру: `docker volume prune --filter "label!=keep"`  
По умолчанию для Docker 17.06.1+ тома не удаляются.  
Чтобы удалились и они тоже: `docker system prune --volumes`


`docker build <path to docker file>`  
`docker build .`  
`docker build github.com/creack/docker-firefox`  

Создает образ из указанного докер-файла


`docker load < ubuntu.tar.gz`  
`docker load --input ubuntu.tar`  

Загрузка докер-образа из репозитория tar (из файла или стандартного ввода)


`docker save busybox > ubuntu.tar`  

Сохранение образа в tar-архив



---

##### Сети

Создание сети:

`docker network create -d overlay MyOverlayNetwork`  
`docker network create -d bridge MyBridgeNetwork`  
```text
docker network create -d overlay \
  --subnet=192.168.0.0/16 \
  --subnet=192.170.0.0/16 \
  --gateway=192.168.0.100 \
  --gateway=192.170.0.100 \
  --ip-range=192.168.1.0/24 \
  --aux-address="my-router=192.168.1.5" --aux-address="my-switch=192.168.1.6" \
  --aux-address="my-printer=192.170.1.5" --aux-address="my-nas=192.170.1.6" \
  MyOverlayNetwork
```

Удаление сети:

`docker network rm MyOverlayNetwork`  

Список сетей:

`docker network ls`  

Получение информации о сети:

`docker network inspect MyOverlayNetwork`  

Подключение работающего контейнера к сети:

`docker network connect MyOverlayNetwork <container id>`  

Подключение контейнера к сети при его запуске:

`docker run -it -d --network=MyOverlayNetwork <container id>`  

Отключение контейнера от сети:

`docker network disconnect MyOverlayNetwork <container id>`  

Удаление неиспользуемых сетей:

`docker network prune`

---

Info:
- https://docs.docker.com/engine/reference/commandline/docker/
- https://docs.docker.com/engine/reference/commandline/cli/
- https://www.edureka.co/blog/docker-commands/
- https://habr.com/ru/companies/flant/articles/336654/
- https://github.com/eon01/DockerCheatSheet

