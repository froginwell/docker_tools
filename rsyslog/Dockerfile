FROM alpine:3.7
RUN wget -q -P /etc/apk/keys http://alpine.adiscon.com/rsyslog@lists.adiscon.com-5a55e598.rsa.pub \
 && echo 'http://alpine.adiscon.com/3.7/stable' >> /etc/apk/repositories \
 && apk --no-cache update \
 && apk add --no-cache \
        rsyslog \
        rsyslog-imptcp
COPY rsyslog.conf /etc/rsyslog.conf
COPY rsyslog.d/*.conf /etc/rsyslog.d/
EXPOSE 514
VOLUME /var/log
ENTRYPOINT ["rsyslogd"]
CMD ["-n", "-f", "/etc/rsyslog.conf"]
