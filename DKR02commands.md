#

```console
docker run -d -p 127.0.0.1:8889:80 --name rbm-dkr-02 -v /home/user/nginx.conf:/etc/nginx/nginx.conf nginx:stable
```

- `-v` или `--volume` - команда используется для создания тома (volume) в Docker, что позволяет пробросить файлы или директории с хост-системы в контейнер.

#

```console
md5sum nginx.conf
```

- `md5sum` - утилита командной строки, используемая для вычисления и проверки 128-битных MD5 хешей файлов.

- `nginx.conf` — имя файла, для которого вычисляется MD5 хеш.

#

```console
docker exec -ti rbm-dkr-02 md5sum /etc/nginx/nginx.conf
```

- `exec` - используется для выполнения команд в уже запущенных контейнерах Docker.

- `-t` или `--tty` - флаг выделяет псевдо-TTY (терминал) для команды в контейнере. Это делает взаимодействие с контейнером более похожим на работу в обычном терминале, предоставляя возможность, например, использовать текстовые редакторы или интерактивные приложения. Он также обеспечивает корректное форматирование и отображение вывода команд.

- `-i` или `--interactive` - флаг обеспечивает интерактивность команды, выполняемой в контейнере. Это означает, что команда будет поддерживать STDIN (стандартный ввод) открытым даже если не подключен терминал. Это полезно, когда нужно предоставить ввод данных команде внутри контейнера.

#