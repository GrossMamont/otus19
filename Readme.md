# otus19 Docker: основы работы с контейнеризацией 
-----------------------------------------------------------------------
### Домашнее задание

1. Установите Docker на хост машину

2. Установите Docker Compose - как плагин, или как отдельное приложение

3. Создайте свой кастомный образ nginx на базе alpine. После запуска nginx должен отдавать кастомную страницу (достаточно изменить дефолтную страницу nginx)

4. Определите разницу между контейнером и образом

5. Ответьте на вопрос: Можно ли в контейнере собрать ядро?

6. Собранный образ необходимо запушить в docker hub и дать ссылку на ваш репозиторий.

#### Установка Docker и Docker Compose (плагин) на хост машину

Настраиваем apt репозиторий Docker.
```sudo apt-get update```
```sudo apt-get install ca-certificates curl```
```sudo install -m 0755 -d /etc/apt/keyrings```
```sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc```
```sudo chmod a+r /etc/apt/keyrings/docker.asc```

```echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null```
```sudo apt-get update```

Установка последнего пакета Docker.
```sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin```

Создадим группу и добавим туда пользователя, чтобы работать не от sudo.
```sudo groupadd docker```
```sudo usermod -aG docker $USER```
```newgrp docker```

Проверка успешной установки.
```sudo docker run hello-world```

- Устаналиваем пререквизиты ```yum install -y yum-utils device-mapper-persistent-data lvm2```
- Добавляем репозиторий ```yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo```
- Устанавливаем Docker и дополнительные компоненты ```yum install docker-ce docker-ce-cli containerd.io```
- Устаналиваем в автозагрузку и стартуем Docker ```systemctl enable docker; systemctl start docker```

Данный алгоритм установки взять с официального сайта Docker.

#### Создать свой кастомный образ nginx на базе alpine. После запуска nginx должен отдавать кастомную страницу (достаточно изменить дефолтную страницу nginx)
```touch Dockerfile```
```touch index.html```

Содержимое Dockerfile
FROM nginx
COPY ./ /usr/share/nginx/html

Содержимое index.html
Hello!!! 
It's my page!

Соберем и запустим с пробросом порта 
```docker build -t my-nginx .```
```docker run --name my-nginx -d -p 8080:80 my-nginx```

Страница [http://localhost:8080/] выводит Hello!!! It's my page!

```
docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                                     NAMES
b88c5d63a031   my-nginx   "/docker-entrypoint.…"   45 minutes ago   Up 45 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx
```



#### Определите разницу между контейнером и образом

1. Определите разницу между контейнером и образом. Вывод опишите в домашнем задании.
- Контейнер Docker – это автономное запускаемое программное приложение или сервис. 
- Образ Docker – это шаблон, загруженный в контейнер для его запуска, например набор инструкций.

Образ Docker - аналогичен образу ВМ, который можно использовать многократно и совместно,
контейнер же представляет собой инстанс приложения или сервиса, запущенного на базе образа.


2. Ответьте на вопрос: Можно ли в контейнере собрать ядро?
- Docker использует хостовое ядро для управления ресурсами.
В самом контейнере собрать ядро можно (по слухам), но использоваться оно не будет.
Пример сборки ядра в контейнере. [https://real-time-working-group.readthedocs.io/en/foxy/Guides/Real-Time-Operating-System-Setup/Real-Time-Linux/build_rt_kernel.html]

#### Собранный образ необходимо запушить в docker hub и дать ссылку на ваш репозиторий.
Образ доступен по ссылке
[https://hub.docker.com/repository/docker/herrmamont/my-nginx/general]

