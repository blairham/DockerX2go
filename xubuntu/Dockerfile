FROM ubuntu:16.04
MAINTAINER Paimpozhil "paimpozhil@gmail.com"

RUN apt-get update 

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get install -y openssh-server xubuntu-desktop 

RUN add-apt-repository ppa:x2go/stable

RUN apt-get update

RUN apt-get install x2goserver x2goserver-xsession pwgen -y

RUN mkdir -p /var/run/sshd && sed -i "s/UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config && sed -i "s/UsePAM.*/UsePAM no/g" /etc/ssh/sshd_config
RUN sed -i "s/PermitRootLogin.*/PermitRootLogin yes/g" /etc/ssh/sshd_config
RUN sed -i "s/#PasswordAuthentication/PasswordAuthentication/g" /etc/ssh/sshd_config

RUN curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
RUN sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
RUN sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
RUN sudo apt-get update
RUN sudo apt-get install code

RUN mkdir -p /tmp/.X11-unix && chmod 1777 /tmp/.X11-unix

ADD set_root_pw.sh /set_root_pw.sh
ADD run.sh /run.sh
RUN chmod +x /*.sh

EXPOSE 22

CMD ["/run.sh"]
