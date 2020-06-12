FROM nginx:alpine

RUN apk update && apk upgrade

RUN apk add openssl
	
RUN openssl req -x509 -newkey rsa:4096 -keyout /etc/nginx/certificate.key -out /etc/nginx/certificate.pem -days 365 -subj '/C=FR/ST=Denial/L=Paris/O=42/CN=localhost' -nodes

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
COPY default.conf /etc/nginx/conf.d/default.conf
# test
COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80 443 22

COPY start.sh /usr/local/bin/
RUN chmod 700 /usr/local/bin/start.sh 

ENTRYPOINT ["sh","/usr/local/bin/start.sh"]
CMD ["nginx", "-g", "daemon off;"]

#&& rm  -rf /tmp/* /var/cache/apk/*

#docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
