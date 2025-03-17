FROM debian:buster

RUN apt-get update -y && \
    apt-get install -y php-fpm php-mysql curl

COPY ./setup.sh /

RUN chmod +x /setup.sh

EXPOSE 9000

CMD ["/setup.sh"]
