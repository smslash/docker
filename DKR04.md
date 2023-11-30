# DKR 04: Basics. Внешнее хранилище

## 1. Настройка конфигурационного файла nginx

Подключаемся к серверу. Создаем `nginx.conf` и заполняем данными из [ссылки](https://gitlab.rebrainme.com/docker-course-students/dkr-nginx-conf-3/blob/master/nginx.conf) и сохраним 

```console
nano nginx.conf
```

## 2. Создание Volume

Создадим volume с именем rbm-dkr-04-volume

```console
docker volume create rbm-dkr-04-volume
```

## 3. Запуск контейнера

Запустим контейнер с указанными параметрами

```console
docker run -d -p 127.0.0.1:8891:80 --name rbm-dkr-04 -v /home/user/nginx.conf:/etc/nginx/nginx.conf -v rbm-dkr-04-volume:/var/log/nginx/external nginx:stable
```

Проверим работу, обратившись к 127.0.0.1:8891

```console
curl http://127.0.0.1:8891
```

В ответе должны поулчить `Welcome to the training program RebrainMe: Docker!`. Выведим список запущенных контейнеров

```console
docker ps
```

Контейнер должен быть запущен. Выведим список существующих docker volumes

```console
docker volume ls
```

В списке будет наш volume. Теперь выведим содержимое volume на хостовой системе, воспользовавшись командой ls -la

```console
sudo ls -la /var/lib/docker/volumes/rbm-dkr-04-volume/
```

Я получил следующий вывод

```console
total 12
drwx-----x 3 root root 4096 Nov 30 15:21 .
drwx-----x 3 root root 4096 Nov 30 15:21 ..
drwxr-xr-x 2 root root 4096 Nov 30 15:24 _data
```