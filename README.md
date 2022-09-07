# flask_app
Простое web-приложение, написано с помощью фреймворка Flask на python3. Разворачиваем на Linux.За основу взята Ubuntu server 22.04.1. В качестве веб-сервера будем использовать Gunicorn - python-friendly веб-сервер. :)
# Установка базовых зависимостей.
Для начала, установим все нужные нам пакеты на сервере с помощью следующих команд:

    $ sudo apt-get -y update
    $ sudo apt-get -y install python3 python3-venv python3-dev
    $ sudo apt-get -y install supervisor nginx git

Затем, убедившись, что находимся в домашней директории нужного нам юзера (который будет все это разворачивать), склонируем репозиторий и зайдем внутрь:

    $ git clone https://github.com/BigYorlde/flask_app.git
    $ cd flask_app/

Далее, создадим виртуальную среду, запустим ее, и установим все необходимые пакеты, которые для удобства записаны в requirements.txt:

    $ python3 -m venv venv
    $ source venv/bin/activate
    (venv) $ pip install -r requirements.txt

Нам понадобится еще один пакет с нашим веб-сервером, который не занесен в requirements, потому как в локальной среде разработки он не нужен. Выполним:

    (venv) $ pip install gunicorn

# Настроим Gunicorn и Supervisor
