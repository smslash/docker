# DKR 08: Images. Введение в Dockerfile

## 1. Создание конфигурационного файла nginx

Создадим файл nginx.conf

```console
nano nginx.conf
```

Скопируем туда данные из [ссылки](https://gitlab.rebrainme.com/docker-course-students/dkr-nginx-conf-1/blob/master/nginx.conf). Сохраним и закроем файл

## 2. Создание Dockerfile

Создадим файл Dockerfile

```console
nano Dockerfile
```

В этом файле укажем следующее содержимое

```console
FROM nginx:stable
COPY nginx.conf /etc/nginx/nginx.conf
```

Здесь мы используем nginx:stable в качестве базового образа и копируем наш nginx.conf в стандартное местоположение для конфигурационных файлов Nginx в контейнере

## 3. Сборка образа

Соберем Docker образ с указанным именем и тегом

```console
docker build -t nginx:rbm-dkr-08 .
```

После сборки образа выведем список всех образов на хосте

```console
docker images
```

Теперь на основе нашего образа запустим контейнер

```console
docker run -d -p 127.0.0.1:8900:80 nginx:rbm-dkr-08
```

Выведем список запущенных контейнеров

```console
docker ps
```

Наш контейнер должен быть в списке. Отправляем решение на проверку