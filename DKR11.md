# DKR 11: Images. Введение в понятие «слои»

## 1. Создание файла и образа

Создадим тестовый файл testfile размером 10МБ

```console
dd if=/dev/zero of=./testfile bs=1M count=10
```

Создадим Dockerfile с указанным содержимым

```console
nano Dockerfile
```

```console
FROM ubuntu:20.04
ENV testenv1=env1
#создадим пользователя
RUN groupadd --gid 2000 user && useradd --uid 2000 --gid 2000 --shell /bin/bash --create-home user
#посмотрим состояние кэша apt до установки nginx
RUN ls -lah /var/lib/apt/lists/
RUN apt-get update -y && apt-get install nginx -y
#Повторно проверим состояние кэша apt
RUN ls -lah /var/lib/apt/lists/
#Очистим кзш
RUN rm -rf /var/lib/apt/lists/*
RUN ls -lah /var/lib/apt/lists/
#Скопируем наш тестовый файл
COPY testfile .
#Сменим права
RUN chown user:user testfile
USER user
CMD ["sleep infinity"]
```

Сохраним и закроем файл. Соберем образ rbm-dkr-11:default

```console
docker build -t rbm-dkr-11:default .
```

Анализ образа с помощью docker inspect

```console
docker inspect rbm-dkr-11:default
```

Анализ слоёв образа с помощью docker history

```console
docker history rbm-dkr-11:default --no-trunc
```

## 2. Модификация Dockerfile

Указанные изменения нужно будет внести в Dockerfile, улучшив его с точки зрения эффективности использования слоёв. Таким образом Dockerfile будет иметь следующий вид

```console
FROM ubuntu:20.04
ENV testenv1=env1
# Создадим пользователя
RUN groupadd --gid 2000 user && useradd --uid 2000 --gid 2000 --shell /bin/bash --create-home user
# Посмотрим состояние кэша apt до установки nginx
RUN ls -lah /var/lib/apt/lists/
# Обновляем и устанавливаем nginx, затем очищаем кэш apt
RUN apt-get update -y && apt-get install nginx -y && rm -rf /var/lib/apt/lists/*
# Повторно проверим состояние кэша apt
RUN ls -lah /var/lib/apt/lists/
# Скопируем наш тестовый файл и назначим пользователя user его владельцем
COPY --chown=user:user testfile .
USER user
CMD ["sleep infinity"]
```

Внесенные изменения:

- Оптимизация установки nginx и очистки кэша apt: Команда `apt-get install nginx -y && rm -rf /var/lib/apt/lists/*` была объединена в одну директиву `RUN`. Это уменьшает количество слоев в образе Docker и очищает кэш apt сразу после установки nginx, что помогает уменьшить общий размер образа.

- Изменение директивы COPY: Использование флага `--chown=user:user` в директиве `COPY` позволяет назначить пользователя `user` владельцем скопированных файлов. Это устраняет необходимость в отдельной директиве `RUN chown`, что также помогает уменьшить количество слоев в образе.

- Удаление директивы `RUN chown`: Так как права на файлы теперь устанавливаются непосредственно в директиве `COPY`, дополнительная команда `chown` больше не нужна.

## 3. Сборка оптимизированного образа

Соберем оптимизированный образ rbm-dkr-11:optimized

```console
docker build -t rbm-dkr-11:optimized .
```

Сравним слои оптимизированного образа

```console
docker history rbm-dkr-11:optimized --no-trunc
```

Сравним размеры итоговых образов

```console
docker images
```

Видно, что rbm-dkr-11:optimized весит меньше Mb. Отправляем решение на проверку