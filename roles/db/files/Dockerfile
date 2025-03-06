FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y mysql-server && \
    apt-get clean

COPY setup.sh /

RUN chmod +x /setup.sh

EXPOSE 3306

CMD [ "/setup.sh" ] 