# DKR 09: Images. Основные директивы Dockerfile

## 1. Написание Dockerfile

Создадим файл Dockerfile

```console
nano Dockerfile
```

В этом файле укажем следующее содержимое

```console
FROM ubuntu:18.04

RUN apt-get update && apt-get install -y nginx

COPY nginx.conf /etc/nginx/nginx.conf

ENTRYPOINT ["nginx", "-g", "daemon off;"]

CMD ["-g", "daemon off;"]

WORKDIR /etc/nginx/

VOLUME /var/lib/nginx
```

Здесь мы используем ubuntu:18.04 как базовый образ, устанавливаем nginx, копируем конфигурационный файл nginx.conf (который должен быть в нашей рабочей директории), задаем ENTRYPOINT и CMD согласно требованиям, устанавливаем рабочую директорию и определяем volume.

## 2. Сборка образа

Соберем Docker образ с указанным именем и тегом

```console
docker build -t nginx:rbm-dkr-09 .
```

Выведем список образов

```console
docker images
```

## 3. Запуск контейнера

Запустим контейнер на основе собранного образа с указанными параметрами

```console
docker run -d -p 127.0.0.1:8901:80 nginx:rbm-dkr-09
```

Выведем список запущенных контейнеров

```console
docker ps
```

Наш контейнер должен быть запущен. Отправим запрос на 127.0.0.1:8901

```console
curl 127.0.0.1:8901
```

Получаем ответ `Welcome to the training program RebrainMe: Docker!` и отправляем решение на проверку