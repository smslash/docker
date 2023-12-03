# DKR 12: Images. Удаление образов

## 1. Загрузка образа

Загрузим образ `nginx:stable-alpine`, чтобы он был доступен локально

```console
docker pull nginx:stable-alpine
```

Добавим новы тега к загруженному образу

```console
docker tag nginx:stable-alpine nginx:rbm-dkr-12
```

Выведем список всех образов

```console
docker images
```

## 2. Удаление образов
 
Удалим образ nginx:stable-alpine

```console
docker rmi nginx:stable-alpine
```

Еще раз выведем список всех образов

```console
docker images
```

Там будет лежать только `nginx:rbm-dkr-12`. Теперь заново скачаем `nginx:stable-alpine`

```console
docker pull nginx:stable-alpine
```

## 3. Сохранение списка образов в файл

```console
docker images | tee /home/user/images.txt
```

Удалим все образы nginx

```console
docker rmi --force $(docker images -q nginx)
```

Повторный вывод списка образов

```console
docker images
```

## 4. Запуск контейнера

Запустим контейнер со следующими параметрами

```console
docker run -d --name rbm-dkr-12 nginx:stable-alpine
```

Удалим образ без флага

```console
docker rmi nginx:stable-alpine
```

Не получится. Теперь попробуем удалить с флагом

```console
docker rmi --force nginx:stable-alpine
```

Выведем список запущенных контейнеров

```console
docker ps
```

Перезапустим контейнер

```console
docker restart rbm-dkr-12
```

Еще раз проверим список запущенных контейнеров

```console
docker ps
```

Наш контейнер должен работать. Отправим решение на проверку