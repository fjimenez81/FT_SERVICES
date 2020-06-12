FROM alpine:3.11.3

RUN apk add openrc

RUN apk update && apk add vim

RUN apk add mysql mysql-client && mysql_install_db --user=mysql --datadir=/var/lib/mysql

#RUN rm etc/my.cnf && rm etc/my.cnf.d/mariadb-server.cnf

ADD my.cnf /etc/my.cnf

#ADD mariadb-server.cnf /etc/my.cnf.d/mariadb-server.cnf

RUN rc-status

RUN touch /run/openrc/softlevel

ADD start.sh /start.sh

RUN chmod 755 start.sh

VOLUME ["/sys/fs/cgroup"]

ENTRYPOINT ["sh", "/start.sh"]
