# DKR 07: Images. Введение в Docker-образы

## 1. Загрузка образа

Загрузим образ nginx:stable-alpine

```console
docker pull nginx:stable-alpine
```

## 2. Добавление тега

Теперь добавим новый тег `rbm-dkr-07` к загруженному образу, оставив имя репозитория без изменений

```console
docker tag nginx:stable-alpine nginx:rbm-dkr-07
```

Выведем список образов

```console
docker images
```

## 3. Запуск контейнера

Запустим контейнер на основе нового тега образа в фоновом режиме

```console
docker run -d nginx:rbm-dkr-07
```

Выведем список запущенных контейнеров

```console
docker ps
```

Наш контейнер должен быть в списке. Отправляем решение на проверку