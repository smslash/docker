# DKR 15 [Optional]: Images. Multistage. Advanced

## 1. Клонирование через HTTPS вместо SSH

```clone
git clone https://gitlab.rebrainme.com/docker-course-students/gocalc.git
```

## 2. Создание и редактирование Dockerfile

Перейдем в каталог репозитория

```clone
cd gocalc
```

Создадим Dockerfile

```clone
nano Dockerfile
```

Заполним Dockerfile

```clone
FROM golang:1.19-alpine
WORKDIR /build
COPY main.go .
ENV GO111MODULE auto
RUN go mod init main && \
    go mod tidy && \
    go build -o app
FROM alpine:3.10.3
COPY --from=0 /build/app /app
ARG SECRET
RUN echo $SECRET > /secret.txt
CMD ["/app"]
```

## 3. Сборка образа

Создадим secret.txt и будем хранить там `verySecret`

```clone
nano secret.txt
```

Теперь соберем Docker образ, передавая значение секрета через --build-arg

```clone
docker build --build-arg SECRET=verySecret -t gocalc .
```

Проверим список образов

```clone
docker images
```

Отправим решение на проверку