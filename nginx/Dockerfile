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
	apk del tzdata && \
	rm -rf /var/cache/apk/*


#---------- SSH ----------
RUN apk add --no-cache openssh-server
COPY sshd_config /etc/ssh/sshd_config
RUN DEBIAN_FRONTEND=noninteractive /usr/bin/ssh-keygen -A && ssh-keygen -t rsa -b 4096 -f  /etc/ssh/ssh_host_key
RUN echo "root:root" | chpasswd


#---------- NGINX ----------

RUN mkdir -p /usr/share/nginx/html

COPY index.html  /usr/share/nginx/html/

RUN rm etc/nginx/conf.d/default.conf

COPY default.conf etc/nginx/conf.d/

#COPY status.conf etc/nginx/conf.d/
#RUN chmod 755 etc/nginx/conf.d/status.conf

RUN mkdir /run/nginx && touch /run/nginx/nginx.pid

EXPOSE 80 443 22

ADD start.sh /start.sh

RUN chmod 755 start.sh

ENTRYPOINT ["sh", "/start.sh"]

CMD ["nginx", "-g", "daemon off;"]


#docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
