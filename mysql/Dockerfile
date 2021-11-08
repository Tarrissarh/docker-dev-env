FROM mysql:8

LABEL maintainer="Tarrissarh <tarrissarh@gmail.com>"

RUN mkdir -p /var/logs/mysql && touch /var/logs/mysql/error.log /var/logs/mysql/general.log /var/logs/mysql/slow.log
RUN chown -R mysql:mysql /var/logs/mysql && chmod -R 777 /var/logs/mysql

COPY ./conf.d /etc/mysql/conf.d
RUN chmod 0444 /etc/mysql/conf.d/*.cnf

USER mysql

EXPOSE 3306

CMD ["mysqld"]