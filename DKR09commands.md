#

```console
FROM ubuntu:18.04

RUN apt-get update && apt-get install -y nginx

COPY nginx.conf /etc/nginx/nginx.conf

ENTRYPOINT ["nginx", "-g", "daemon off;"]

CMD ["-g", "daemon off;"]

WORKDIR /etc/nginx/

VOLUME /var/lib/nginx
```

- `FROM ubuntu:18.04` - инструкция задаёт базовый образ. Здесь используется Ubuntu 18.04.

- `RUN apt-get update && apt-get install -y nginx` - команда обновляет список пакетов и устанавливает Nginx. Флаг -y используется для автоматического подтверждения установки.

- `COPY nginx.conf /etc/nginx/nginx.conf` - копирует файл nginx.conf из вашей локальной директории (где находится Dockerfile) в образ, заменяя стандартный файл конфигурации Nginx.

- `ENTRYPOINT ["nginx", "-g", "daemon off;"]` - устанавливает исполняемый файл и параметры, которые будут выполнены при запуске контейнера. Здесь он запускает Nginx с параметром -g, указывающим Nginx работать в режиме переднего плана (daemon off;).

- `CMD ["-g", "daemon off;"]` - устанавливает параметры по умолчанию, которые могут быть перезаписаны при запуске контейнера. Однако, поскольку ENTRYPOINT уже задан, CMD будет игнорироваться, если только не будет предоставлены дополнительные параметры при запуске контейнера.

- `WORKDIR /etc/nginx/` - устанавливает рабочую директорию для любых последующих RUN, CMD, ENTRYPOINT, COPY и ADD инструкций. В этом случае, рабочая директория устанавливается на /etc/nginx/.

- `VOLUME /var/lib/nginx` - создаёт точку монтирования с указанным путем. Это используется для хранения данных и состояний, которые должны быть сохранены независимо от контейнера. В данном случае, это /var/lib/nginx.

#