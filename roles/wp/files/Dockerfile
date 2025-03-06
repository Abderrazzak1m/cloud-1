FROM ubuntu:latest

RUN apt update -y
RUN apt update -y && apt update -y \
    && apt install -y php-fpm php-mysql
RUN apt install curl -y

COPY ./setup.sh /

RUN chmod +x ./setup.sh

EXPOSE 9000

CMD ["/setup.sh"]