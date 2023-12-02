# DKR 10: Images. Параметризация Dockerfile

## 1. Создание Dockerfile файл

Создадим докер файл

```console
nano Dockerfile
```

И заполним следующими данными

```console
ARG NG_VERSION
FROM nginx:${NG_VERSION}
ENV NG_VERSION=${NG_VERSION}
ARG ARG_FILE
RUN touch /opt/${ARG_FILE}
```

Сохраним и закроем файл

## 2. Сборка образа

Замените <your_ng_version> и <your_arg_file> на соответствующие значения.

```console
docker build --build-arg NG_VERSION=<your_ng_version> --build-arg ARG_FILE=<your_arg_file> -t nginx:rbm-dkr-10 /home/user/
```

Лично я использовал `1.19.3` и `testfile`. Выведем список образов

```console
docker images
```

## 3. Запуск контейнера Docker

Запустим контейнер со следующими параметрами

```console
docker run -d --name rbm-dkr-10 -e REBRAINME=DKR10 nginx:rbm-dkr-10
```

Выведем список запущенных контейнеров

```console
docker ps
```

Просмотрим на переменные окружения в контейнере

```console
docker exec rbm-dkr-10 env
```

Просмотрим файлы в директории /opt/

```console
docker exec rbm-dkr-10 ls /opt/
```

Там будет лежать наш testfile. Отправим решение на проверку