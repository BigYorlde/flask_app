# Взять образ с установленным питоном на базе linux alpine за основу
FROM python:3.7.14-alpine
# Задаем директорию по-умолчанию
WORKDIR /home/flask_app
# Копируем с локальной машины в контейнер
COPY . .
# Запускаем shell-команды для установки зависимостей
RUN python -m venv venv && \
venv/bin/pip install -r requirements.txt && \
venv/bin/pip install gunicorn
# Какая команда будет выполнена при запуске контейнера.
CMD exec venv/bin/gunicorn -b :8000 --access-logfile - --error-logfile - -w 2 application:application

