# DKR 01: Basics. Знакомство с Docker

## 1. Установка Docker:

Подключаемся к виртуальной машине

```console
ssh user@128.199.44.148
```

Терминал спросит **Are you sure you want to continue connecting (yes/no/[fingerprint])?** 
Отвечаем **yes**

После нужно будет ввести пароль который предоставит Rebrain. По первым строчкам можно понять, что ОС является Ubuntu (Linux)

```console
Welcome to Ubuntu 20.04.1 LTS (GNU/Linux 5.4.0-51-generic x86_64)
```

Сначала проверим установлен ли Docker

```console
docker --version
```

Выдает следующее

```console
Command 'docker' not found, but can be installed with:

sudo snap install docker     # version 20.10.24, or
sudo apt  install docker.io  # version 19.03.8-0ubuntu1.20.04.2
```

Таким образом ВМ (Виртуальная Машина) подсказала как можно установить Docker (можно выбрать любую версию). После установки выдает следующее

```console
user@server:~$ sudo snap install docker
docker 20.10.24 from Canonical✓ installed
```

При необходимости можно установить последнюю версию *Docker* следующим образом

```console
sudo apt install docker-ce docker-ce-cli containerd.io
```

## 2. Запуск и остановка контейнера:

> Дальше перед каждой командой *Docker* будет использоваться команда `sudo`. Использование команды `sudo` перед другими командами предоставляет вам административные привилегии для их выполнения. Но можно обойтись без него. Можно добавить своего пользователя в группу *docker* с помощью команды: `sudo usermod -aG docker ваше_имя_пользователя`. В данном случае вместо `ваше_имя_пользователя` будет `user`. После этого можно не писать каждый раз `sudo`.

Запускаем контейнер на порту `28080`

```console
sudo docker run -d -p 127.0.0.1:28080:80 --name rbm-dkr-01 nginx:stable
```

Посмотрим запущен ли он

```console
sudo docker ps
```

В списке будет наш контейнер с именем `rbm-dkr-01`. Отправим запрос на http://127.0.0.1:28080

```console
sudo curl http://127.0.0.1:28080
```

Появится приветственная страница nginx. Теперь остановим контейнер

```console
sudo docker stop rbm-dkr-01
```

Еще раз посмотрим запущен ли он

```console
sudo docker ps
```

В списке уже не будет нашего контейнера. Повторно отправим запрос на http://127.0.0.1:28080

```console
sudo curl http://127.0.0.1:28080
```

Появилась ошибка, поскольку мы уже остановили контейнер и ничего не слушается на этом порту. Выведем список всех контейнеров в системе

```console
sudo docker ps -a
```

В списке будет лежать наш остановленный контейнер