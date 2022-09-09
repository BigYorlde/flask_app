FROM python:3.7.14-alpine

WORKDIR /home/flask_app

COPY . .

RUN python -m venv venv && \
venv/bin/pip install -r requirements.txt && \
venv/bin/pip install gunicorn && \
venv/bin/gunicorn -D -b localhost:8000 -w 2 application:application

