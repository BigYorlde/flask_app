# flask_app
Простое web-приложение, написано с помощью фреймворка Flask на python3. Разворачиваем на Linux.За основу взята Ubuntu server 22.04.1. В качестве веб-сервера будем использовать Gunicorn - python-friendly веб-сервер. :) В качестве публичного веб-сервера развернем Nginx.
# Установка базовых зависимостей.
Для начала, установим все нужные нам пакеты на сервере с помощью следующих команд:

    $ sudo apt-get -y update
    $ sudo apt-get -y install python3 python3-venv python3-dev
    $ sudo apt-get -y install supervisor nginx git

Затем, убедившись, что находимся в домашней директории нужного нам юзера (который будет все это разворачивать), склонируем репозиторий и зайдем внутрь:

    $ git clone -b main --single-branch https://github.com/BigYorlde/flask_app.git ~/flask_app
    $ cd flask_app/

Далее, создадим виртуальную среду, запустим ее, и установим все необходимые пакеты, которые для удобства записаны в requirements.txt:

    $ python3 -m venv venv
    $ source venv/bin/activate
    (venv) $ pip install -r requirements.txt

Нам понадобится еще один пакет с нашим веб-сервером, который не занесен в requirements, потому как в локальной среде разработки он не нужен. Выполним:

    (venv) $ pip install gunicorn

# Настроим Gunicorn и Supervisor
Для запуска приложения под gunicorn используем:

    (venv) $ gunicorn -b localhost:8000 -w 2 application:application
    # -b localhost:8000 - установлен порт 8000 внутреннего сетевого интерфейса для прослушки запросов.
    # -w 2 - сколько процессов может работать одновременно, т.е. сколько клиентов может получить контент в одно время
    # application:application - файл, содержащий приложение : имя этого приложения

Запуск gunicorn в фоновом режиме без привязки к сессии юзера с помощью Supervisor:

1.   Возьмите файл flask_app.conf и переместите в директорию утилиты - /etc/supervisor/conf.d/

2.   Сделайте $ sudo supervisorctl reload
    
    
# Настройка Nginx
*Необзятательно, но мне захотелось :)* Перенаправим весь трафик с 80 порта на 443, т.е. сделаем весь трафик по https. Сделаем сертики:

    $ mkdir certs
    $ openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -keyout certs/key.pem -out certs/cert.pem
    
Удаляем default страницу в Nginx:
    
    $ sudo rm /etc/nginx/sites-enabled/default
    
И помещаем наш конфиг nginx/flask_app в эту директорию.
Делаем reload:

    $ sudo service nginx reload
    
Мои поздравления! Сайт доступен по <ip of your VM>
    
# Обновляем сайт
Обновить наш сайт можно следующим образом:

    (venv) $ git pull                              # Скачать новую версию из этого репозитория
    (venv) $ sudo supervisorctl stop flask_app     # Остановка текущего сервера
    (venv) $ sudo supervisorctl start flask_app    # Запуск нового сервера

