FROM alpine
RUN adduser -D -u 1001 -g 'www' www
RUN apk update && apk add nginx 
RUN mkdir /app && \
    mkdir /run/nginx && \
    chown -R www:www /run/nginx && \
    chown -R www:www /var/lib/nginx && \
    chown -R www:www /var/log/nginx && \
    chown -R www:www /var/tmp/nginx && \
    chown -R www:www /app
ADD nginx.conf /etc/nginx/nginx.conf
USER 1001
EXPOSE 8000
ENTRYPOINT [ "nginx" ]
CMD [ "-c", "/etc/nginx/nginx.conf"]