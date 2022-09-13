# Берем образ Nginx из dockerhub
FROM nginx
# Копируем наш конфиг в контейнер
COPY ./templates /etc/nginx/templates
# Копируем self-signed сертификаты для ssl трафика
COPY ./certs /etc/nginx/certs

