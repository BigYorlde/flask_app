# flask_app в Docker
Шпоргалка предполагает, что докер уже установлен на железо, и Вы знаете, что такое Dockerfile и для чего он нужен, docker image, поэтому мы опустим этот момент.
С помощью Docker установим Nginx и наше приложение.
Начнем со скачивания нашего бранча docker этого репозитория на локальную машину:

    $ git clone -b docker --single-branch https://github.com/BigYorlde/flask_app.git ~/flask_app

# Создадим images Nginx и нашего flask_app

Для того, чтобы создать image своего приложения, необходимо выполнить:

    $ docker build -t flask_app:1.0 ~/flask_app/app   # В конце путь до Dockerfile

Для image nginx выполним это:

    $ docker build -t nginx_server:1.0 ~/flask_app/configs/nginx

Проверить, что images создались можно командой:

    $ docker images    

# Запустим наши контейнеры
## С помощью Docker
Шаг 1: Создаем общую сеть для двух контейнеров:

    $ docker network create flask

Шаг 2: Запускаем контейнеры

    $ docker run -d -p 8000:8000 --net flask_app --name flask flask_app:1.0
    $ docker run -d -p 80:80 -p 443:443 -e NGINX_HOST=flask -e NGINX_PORT=80 --net flask_app --name nginx_server nginx_server:1.0

-d - запуск контейнера в фоновом режиме

-p - открыть и связать порты в формате local_port:container_port

-e - переменные окружения

--net - задать сеть, в которой будет работать контейнер (создали ее в Шаг 1)

--name - имя контейнера

Шаг 3: Проверьте работоспособность

    $ docker ps  # просмотр запущенных контейнеров

И попробовать зайти на любую страничку нашего сайта:

    http://<ip address of your machine>/
    
## С помощью Docker-compose
Запускаем docker-compose, используя наш конфиг файл:

    $ docker-compose -f ~/flask_app/flask_app.yaml up -d

Можно проверять, поднялись ли контейнеры и доступен ли сайт.