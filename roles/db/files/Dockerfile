
FROM debian:buster

RUN apt-get update
RUN apt-get install mariadb-server -y

COPY ./setup.sh /

RUN chmod +x /setup.sh

EXPOSE 3306

CMD [ "/setup.sh" ]