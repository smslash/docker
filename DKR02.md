# DKR 02: Basics. Флаги запуска

## 1. Подключение

Подключаемся к виртуальной машине

```console
ssh user@134.122.49.203
```

Терминал спросит **Are you sure you want to continue connecting (yes/no/[fingerprint])?** 
Отвечаем **yes**. Вводим пароль для входа.

## 2. Настройка nginx.conf

Создаем и настраиваем файл *nginx.conf*

```console
nano nginx.conf
```

Будем хранить данные которые возьмем из [конфигурационного файла nginx](https://gitlab.rebrainme.com/docker-course-students/dkr-nginx-conf-1/blob/master/nginx.conf)

Сохраняем и закрываем файл *nginx.conf*

## 3. Запуск контейнера rbm-dkr-02

```console
docker run -d -p 127.0.0.1:8889:80 --name rbm-dkr-02 -v /home/user/nginx.conf:/etc/nginx/nginx.conf nginx:stable
```

Проверяем список запущенных контейнеров

```console
docker ps
```

Отправляем запрос на 127.0.0.1:8889

```console
curl 127.0.0.1:8889
```

Получаем ответ *Welcome to the training program RebrainMe: Docker!*

## 4. Сравниваем подсчет md5sum

Сравним md5sum нашего nginx.conf и /etc/nginx/nginx.conf.

Наш nginx.conf:

```console
md5sum nginx.conf
```

/etc/nginx/nginx.conf:

```console
docker exec -ti rbm-dkr-02 md5sum /etc/nginx/nginx.conf
```

Убеждаемся, что совпадает

> ВАЖНО: Если не совпало, то переправьте nginx.conf файл. Данные которые там лежат должны полностью совпадать с файлом из ссылки. (Каждый оступ, каждый пробел)

Не останавливая контейнер, отправляем задание на проверку