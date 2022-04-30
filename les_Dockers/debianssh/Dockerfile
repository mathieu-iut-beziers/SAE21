FROM debian:latest

RUN apt-get update -y \
	&& apt-get upgrade -y \
	&& apt-get install -y apt-utils \
	&& apt-get install -y iputils-ping \
	&& apt-get install -y iptables \
	&& apt-get install -y isc-dhcp-client \
	&& apt-get install -y nano \
	&& apt-get install -y iproute2 \
	&& apt-get install -y net-tools \
    	&& apt-get install -y openssh-server \
    	&& echo Port 22 >> /etc/ssh/ssh_config \
    	&& echo Port 22 >> /etc/ssh/sshd_config \
    	&& useradd -ms /bin/bash test -p test \
