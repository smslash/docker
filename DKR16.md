# DKR 16: Images. Использование Multistage для генерации нескольких образов

## 1. Клонирование репозитория Grafana

Сначала клонируем репозиторий Grafana из ветки v6.3.x

```console
git clone -b v6.3.x https://github.com/grafana/grafana.git
```

## 2. Изменение Dockerfile

Нужно добавить новый этап в Dockerfile для создания образа nginx, который будет содержать скомпилированные статические файлы

```console
cd grafana
```

Редактируем Dockerfile

```console
nano Dockerfile
```

В конец файла добавим следующий этап

```console
FROM nginx:alpine AS grafana-nginx
COPY --from=node /usr/src/app/public /usr/share/nginx/html
```

## 3. Сборка образов

Теперь соберем два образа Docker

```console
docker build -t grafana:app .
docker build --target grafana-nginx -t grafana:static .
```

Выведем список образов

```console
docker images
```

Отправляем решение на проверку (у меня оценка 4)

Checking that /home/user/grafana/Dockerfile exists: OK
Checking usage nginx:alpine: OK
Checking that static have COPY public folder from previous stage : FAILED
Checking that image grafana:app exists: OK
Checking that image grafana:static exists: OK

Если получите оценку 5 напишите мне в лс плз