FROM nginx:1.23.1

COPY html /usr/share/nginx/html

RUN rm -r /etc/nginx/conf.d/default.conf

COPY conf/backend_server.conf /etc/nginx/conf.d/
