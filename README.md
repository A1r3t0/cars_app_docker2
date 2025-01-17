# Мониторинг нагрузки контейнера (Артюшин Артём ФТ-320008)

**Dockerfile и сам проект находятся в этом репозитории: https://github.com/A1r3t0/cars-app-docker**

Для выполнения задания №3 я не использовал встроенные в docker средства мониторинга нагрузки (из Docker Desktop), использовал для этого Portainer.

Portainer — проект с открытым исходным кодом, представляющий собой образ графического web-интерфейса для управления Docker. Помимо предоставления полной информации об установленном Docker, Portainer позволяет управлять всеми сущностями Docker, включая управление контейнерами, образами и сетями.

Для развертывания portainer необходимо иметь docker, у меня он уже установлен.

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.13.49.png)

Сначала необходимо создать том (volume), где будет хранилище данных Portainer:

`docker volume create portainer_data`

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.14.43.png)

После создания хранилища необходимо запустить сам Portainer командой:

`docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce`

Данная команда установит Portainer CE, настроит автозапуск контейнера после перезагрузки и укажет постоянное хранилище для настроек, которые не потеряются при удалении, обновлении или повторном развертывании. Порт контейнера перенаправлен на порт 9000 хоста (по умолчанию стоит 9443).

Чтобы убедится, что Portainer запущен выполним команду `docker ps`

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.15.40.png)

У меня есть web-приложение, представляющее собой веб-интерфейс для бронирования автомобилей в аренду,  создадим его образ с помощью команды:

`sudo docker build -t my-project:cars-app-project .` 

, для этого положим всё необходимое (включая dockerfile) в одну директорию.

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.16.18.png)

Переходим во вкладку Containers, создаём контейнер, устанавливая название, порт, выбирая образ, деплоим его и запускаем кнопкой "Start".

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.16.59.png)

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.17.36.png)

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.18.15.png)

Откроем приложение в браузере. Как можно заметить, оно запускается без проблем.

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.18.37.png)

При выполнении действий на сайте (контейнере) у нас появляется нагрузка, её можно просмотреть в настройках контейнера, перейдя в раздел “Stats”.

![img](https://github.com/A1r3t0/cars_app_docker2/blob/main/Screenshot%202025-01-17%20at%2011.18.55.png)
