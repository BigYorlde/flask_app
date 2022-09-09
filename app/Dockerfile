FROM python:3.7.14-alpine

WORKDIR /home/flask_app

COPY . .

RUN python -m venv venv && \
venv/bin/pip install -r requirements.txt && \
venv/bin/pip install gunicorn

CMD venv/bin/gunicorn -b :8000 -w 2 application:application

