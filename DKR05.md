# DKR 05: Basics. Остановка, удаление контейнеров

## 1. Запуск двух контейнеров с генерацией случайных имен

Выполним следующую команду дважды для запуска двух контейнеров с разными случайными именами

```console
docker run -d --name rbm-dkr-05-run-$(cat /dev/urandom | tr -cd 'a-f0-9' | head -c 10) nginx:stable
```

Запустим еще один контейнер

```console
docker run -d --name rbm-dkr-05-stop nginx:stable
```

Перенаправим вывод команды docker ps в файл

```console
docker ps | tee /home/user/ps.txt
```

## 2. Остановка и удаление контейнера

Остановим контейнер rbm-dkr-05-stop

```console
docker stop rbm-dkr-05-stop
```

Выведим список всех контейнеров

```console
docker ps -a
```

Остановим все запущенные контейнеры одной командой

```console
docker stop $(docker ps -q)
```

Повторно выведим список всех контейнеров

```console
docker ps -a
```

Теперь удалим все контейнеры одной командой

```console
docker rm $(docker ps -a -q)
```

Отправим решение на проверку