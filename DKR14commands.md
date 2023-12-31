#

```console
ssh-keygen -t rsa -b 4096 -C "ali.tlekbai@gmail.com"
```

- `ssh-keygen` - программа, используемая для создания новых ключей SSH. SSH (Secure Shell) используется для безопасного подключения к удаленным машинам и системам.

- `-t rsa` - флаг `-t` (тип) задает алгоритм шифрования для ключа. В данном случае, используется `rsa`, что является наиболее распространенным и рекомендуемым выбором. RSA (Rivest–Shamir–Adleman) — это алгоритм асимметричного шифрования, который используется для создания пары ключей: открытого и закрытого.

- `-b 4096` - флаг `-b` (бит) указывает на размер ключа. Значение `4096` бит обеспечивает хороший уровень безопасности. Это длиннее и безопаснее, чем более стандартные 2048 бит, и рекомендуется для современных приложений.

- `-C` - флаг (комментарий) позволяет добавить к ключу комментарий для его идентификации. Обычно здесь указывается адрес электронной почты, который помогает идентифицировать, кому принадлежит ключ. В данном случае, комментарий — это "ali.tlekbai@gmail.com".

#

```console
FROM golang:1.19-alpine AS builder
WORKDIR /app
COPY main.go .
ENV GO111MODULE auto 
RUN go mod init main && \
    go mod tidy && \
    go build -o app
FROM alpine:3.10.3
COPY --from=builder /app/app /app
CMD ["/app"]
```

- `FROM golang:1.19-alpine AS builder` - начало стадии сборки, где используется образ `golang` версии `1.19` на основе `alpine` в качестве базового. `AS builder` дает этой стадии имя "builder", чтобы можно было на нее ссылаться позже.

- `WORKDIR /app` - устанавливает рабочую директорию внутри контейнера в `/app`. Все следующие команды будут выполняться в этой директории.

- `COPY main.go .` - копирует файл `main.go` из вашей локальной директории в рабочую директорию контейнера `(/app)`.

- `ENV GO111MODULE auto ` - устанавливает переменную окружения `GO111MODULE` в значение `auto`, что включает систему модулей Go.

- `RUN go mod init main && go mod tidy && go build -o app` - инициализирует новый модуль Go с именем `main`, очищает модуль от неиспользуемых зависимостей и компилирует исходный код Go в исполняемый файл с именем `app`.

- `FROM alpine:3.10.3` - начинается новая стадия сборки на основе легковесного образа `alpine` версии `3.10.3`. Это "чистый" образ, используемый для запуска скомпилированного приложения.

- `COPY --from=builder /app/app /app` - копирует скомпилированный исполняемый файл `app` из стадии `builder` в рабочую директорию текущей стадии.

- `CMD ["/app"]` - задает команду по умолчанию для запуска при создании контейнера из этого образа. В данном случае, это исполняемый файл `/app`.

#

```console
docker rmi --force $(docker images -q nginx)
```

- `git add Dockerfile` - добавляет файл `Dockerfile` в индекс Git. Это первый шаг, чтобы отслеживать изменения в этом файле.

- `git commit -m "ADD: Dockerfile"` - создаёт новый коммит с текущими изменениями в индексе. Опция `-m` позволяет указать сообщение коммита непосредственно в командной строке. Здесь сообщение коммита — `"ADD: Dockerfile"`, что обычно указывает на добавление нового файла `Dockerfile`.

- `git remote add upstream git@gitlab.rebrainme.com:docker_users_repos/2433/dkr-14-gocalc.git` - добавляет новый удаленный репозиторий Git под именем `upstream`. Это адрес репозитория на GitLab, куда вы будете отправлять ваши изменения. `upstream` — это просто имя, которое вы присваиваете этому удаленному репозиторию, чтобы ссылаться на него в будущем. Вы можете использовать любое другое имя. URL `git@gitlab.rebrainme.com:docker_users_repos/2433/dkr-14-gocalc.git` указывает на расположение удаленного репозитория на серверах GitLab.

- `git push upstream master` - отправляет ваши изменения (коммиты) в ветку `master` удаленного репозитория, который вы только что добавили как `upstream`. `git push` — это команда для отправки локальных изменений в удаленный репозиторий. upstream указывает, что изменения должны быть отправлены в удаленный репозиторий, который вы назвали `upstream`. `master` — это имя ветки, в которую вы отправляете изменения.

#