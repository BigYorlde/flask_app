FROM nginx  # Берем образ Nginx из dockerhub
# Копируем наш конфиг в контейнер
COPY ./templates /etc/nginx/templates
# Копируем self-signed сертификаты для ssl трафика
COPY ./certs /etc/nginx/certs

