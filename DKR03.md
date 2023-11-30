# DKR 03: Basics. Запуск команд внутри контейнеров

## 1. Настройка конфигурационного файла nginx

Подключаемся к серверу. Создаем `nginx.conf` и заполняем данными из [ссылки](https://gitlab.rebrainme.com/docker-course-students/dkr-nginx-conf-1/blob/master/nginx.conf) и сохраняем 

```console
nano nginx.conf
```

## 2. Запуск контейнера

Запускаем контейнер со следующими параметрами

```console
docker run -d -p 127.0.0.1:8890:80 --name rbm-dkr-03 -v /home/user/nginx.conf:/etc/nginx/nginx.conf nginx:stable
```

Сравним два хеша нашего файла и `/etc/nginx/nginx.conf`

```console
md5sum nginx.conf

docker exec -ti rbm-dkr-03 md5sum /etc/nginx/nginx.conf
```

Отправим запрос на http://127.0.0.1:8890

```console
curl 127.0.0.1:8890
```

## 3. Настройка нового конфиг файла

Создадим новый конфиг файл на нашей стороне и заполним данными из новой [ссылки](https://gitlab.rebrainme.com/docker-course-students/dkr-nginx-conf-2/blob/master/nginx.conf)

```console
nano new_nginx.conf
```

Копируем данные в nginx.conf

```console
sudo cp new_nginx.conf nginx.conf
```

Перезагрузим nginx без остановки контейнера

```console
docker exec rbm-dkr-03 nginx -s reload
```

Проверим хеши еще раз

```console
md5sum nginx.conf

docker exec -ti rbm-dkr-03 md5sum /etc/nginx/nginx.conf
```

Проверим запущенные контейнеры

```console
docker ps
```

Наш контейнер должен быть запущен