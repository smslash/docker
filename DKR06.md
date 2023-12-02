# DKR 06: Basics. Логирование

## 1. Запуск контейнера

Запустим контейнер с именем rbm-dkr-06-local и настройками логирования

```console
docker run -d -p 127.0.0.1:8892:80 --name rbm-dkr-06-local --log-driver local --log-opt max-size=10mb nginx:stable
```

Выведим список запущенных контейнеров

```console
docker ps
```

Видим, что контейнер запущен. Обратимся к запущенному Nginx для записи логов

```console
curl --silent http://127.0.0.1:8892 > /dev/null
```

Сначала нужно найти путь к файлу логов

```console
docker inspect rbm-dkr-06-local
```

Из вывода команды видно, что поле LogPath пусто.

## 2. Настройка сохранения логов

Настроим глобальное сохранение логов

```console
sudo nano /etc/docker/daemon.json
```

Заполним

```console
{
  "log-driver": "local",
  "log-opts": {
    "max-size": "10m"
  }
}
```

Сохраним и закроем файл. Затем перезапустим Docker

```console
sudo systemctl restart docker
```

## 3. Запуск второго контейнера

Запустим ещё один контейнер rbm-dkr-06-global

```console
docker run -d -p 127.0.0.1:8893:80 --name rbm-dkr-06-global nginx:stable
```

Выведим список запущенных контейнеров снова

```console
docker ps
```

Обратимся к второму запущенному Nginx для записи логов

```console
curl --silent http://127.0.0.1:8893 > /dev/null
```

## 4. Вывод содержимое файла логов второго контейнера

Выведем содержимое файла логов второго контейнера

```console
docker inspect rbm-dkr-06-global
```

Опять LogPath пустой. Отправьте решение на проверку