# DKR 14: Images. Multistage

## 1. Клонирование репозитория gocalc

Проверим SSH ключ

```console
cat ~/.ssh/id_rsa.pub
```

Его у меня не было, поэтому создадим новый SSH-ключ

```console
ssh-keygen -t rsa -b 4096 -C "ali.tlekbai@gmail.com"
```

Скопируeм содержимое файла `~/.ssh/id_rsa.pub`. Перейдем на сайт GitLab и войдем в аккаунт. Вставим наш ключ в поле "Key" и добавим его. После добавления ключа в GitLab, попробуем снова склонировать репозиторий

```console
git clone git@gitlab.rebrainme.com:docker-course-students/gocalc.git
```

> *Использование HTTPS вместо SSH.* Если SSH по-прежнему не работает, можно использовать HTTPS для клонирования репозитория. Это может быть альтернативой, если возникают проблемы с настройкой SSH

```console
git clone https://gitlab.rebrainme.com/docker-course-students/gocalc.git
```

## 2. Создание и редактирование Dockerfile

```console
cd gocalc
nano Dockerfile
```

Добавим в Dockerfile следующее содержимое

```console
# Стадия сборки
FROM golang:1.19-alpine AS builder
WORKDIR /app
COPY main.go .
ENV GO111MODULE auto 
RUN go mod init main && \
    go mod tidy && \
    go build -o app

# Стадия исполнения
FROM alpine:3.10.3
COPY --from=builder /app/app /app
CMD ["/app"]
```

## 3. Сборка образа

Соберем Docker образ с помощью следующей команды

```console
docker build -t dkr-14-gocalc .
```

Посмотрим на список образов

```console
docker images
```

Посмотрим истории сборки образа

```console
docker history dkr-14-gocalc
```

## 4. Push результата в GitLab

```console
git add Dockerfile
git commit -m "ADD: Dockerfile"
git remote add upstream git@gitlab.rebrainme.com:docker_users_repos/2433/dkr-14-gocalc.git
git push upstream master
```

После выполнения команды push, убедитесь, что изменения появились в вашем репозитории на GitLab. Отправим решение на проверку