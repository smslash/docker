#

```console
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

- `FROM alpine:3.10.3` - начинается новая стадия на основе легковесного образа `alpine` версии `3.10.3`.

- `COPY --from=0 /build/app /app` - копирует скомпилированный исполняемый файл `app` из стадии сборки (индексируемой как `0`) в рабочую директорию текущей стадии.

- `ARG SECRET` - определяет аргумент сборки `SECRET`, который можно установить во время сборки образа (например, через `--build-arg SECRET=value`).

- `RUN echo $SECRET > /secret.txt` - выполняет команду, которая записывает значение аргумента `SECRET` в файл `/secret.txt` внутри образа.

- `CMD ["/app"]` - задает команду по умолчанию для запуска при создании контейнера из этого образа, в данном случае, запуск исполняемого файла `/app`.

#