FROM alpine

RUN apk update && apk upgrade && apk add nginx

RUN apk add openssl

#-----------OPENRC------------------
RUN apk add --no-cache openrc

#-----------SSL--------------------

# Timezone
ENV TIMEZONE Europa/Madrid
RUN	apk add tzdata && \
	ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
	echo "${TIMEZONE}" > /etc/timezone && \
	mkdir /etc/nginx/certificates && \
	openssl req \
		-x509 \
		-newkey rsa:2048 \
		-keyout /etc/nginx/certificates/key.pem \
		-out /etc/nginx/certificates/cert.pem \
		-days 365 \
		-nodes \
		-subj /CN=localhost && \
	#mkdir /www && \
	apk del tzdata && \
	rm -rf /var/cache/apk/*



#RUN openssl req -x509 -newkey rsa:4096 -keyout /etc/nginx/certificate.key -out /etc/nginx/certificate.pem -days 365 -subj '/C=FR/ST=Denial/L=Paris/O=42/CN=localhost' -nodes

#RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/certs/localhost.key -out /etc/ssl/certs/localhost.crt

#---------- SSH ----------
RUN apk add --no-cache openssh-server
COPY sshd_config /etc/ssh/sshd_config
RUN DEBIAN_FRONTEND=noninteractive /usr/bin/ssh-keygen -A && ssh-keygen -t rsa -b 4096 -f  /etc/ssh/ssh_host_key
RUN echo "root:root" | chpasswd
# ssh-keygen / ssh sshuser@IP -p PORT -i id_rsa
#RUN apk add --no-cache openssh-server
#COPY key/id_rsa /etc/ssh/ssh_host_rsa_key
#RUN chmod 700 /etc/ssh/ssh_host_rsa_key
#RUN addgroup sshgroup && adduser -Ds /bin/sh -g sshgroup sshuser
#RUN echo 'sshuser:root' | chpasswd
#ARG home=/home/sshuser/
#RUN mkdir $home/.ssh
#COPY key/id_rsa.pub $home/.ssh/authorized_keys
#RUN chown sshuser:sshgroup $home/.ssh/authorized_keys && \
    #chmod 600 $home/.ssh/authorized_keys




#---------- NGINX ----------

RUN mkdir -p /usr/share/nginx/html

COPY index.html  /usr/share/nginx/html/

RUN rm etc/nginx/conf.d/default.conf

COPY default.conf etc/nginx/conf.d/

RUN mkdir /run/nginx && touch /run/nginx/nginx.pid

EXPOSE 80 443 22

ADD start.sh /start.sh

RUN chmod 755 start.sh

ENTRYPOINT ["sh", "/start.sh"]

#&& rm  -rf /tmp/* /var/cache/apk/*

#docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
