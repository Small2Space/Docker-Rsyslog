FROM rockylinux/rockylinux:8.4
MAINTAINER smallru8

ENV SQL_SERVER="127.0.0.1"
ENV SQL_DB="Syslog"
ENV SQL_USER="rsyslog"
ENV SQL_PASSWD="rsyslog"

RUN dnf update -y
RUN dnf install rsyslog rsyslog-mysql mysql -y

RUN echo "module(load=\"ommysql\")" >> /etc/rsyslog.conf
RUN echo "*.*:ommysql:${SQL_SERVER},${SQL_DB},${SQL_USER},${SQL_PASSWD}" >> /etc/rsyslog.conf
RUN echo "$ModLoad imudp" >> /etc/rsyslog.conf
RUN echo "$UDPServerRun 514" >> /etc/rsyslog.conf
RUN echo "$ModLoad imtcp" >> /etc/rsyslog.conf
RUN echo "$InputTCPServerRun 514" >> /etc/rsyslog.conf

COPY createDB.sql /tmp/createDB.sql
RUN mysql -h ${SQL_SERVER} -u ${SQL_USER} -p${SQL_PASSWD} ${SQL_DB} < /tmp/createDB.sql; exit 0

EXPOSE 514
EXPOSE 514/udp

CMD rsyslog start