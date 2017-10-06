FROM centos:7
MAINTAINER Blair Hamilton <blairham@me.com>

# Install ssh service
RUN yum install password openssh-server -y &&\
    sed -ri 's/^UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config &&\
    ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key &&\
    ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key &&\
    ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key

# # Create a user
# RUN groupadd "$GROUP" &&\
#     useradd -g "$GROUP" "$USER" -d /home/"$USER" &&\
#     mkdir /home/"$USER"/.ssh &&\
#     cp /root/authorized_keys /home/"$USER"/.ssh/authorized_keys &&\
#     echo "${PASSWORD}" | passwd "$USER" --stdin &&\
#     echo "${PASSWORD}" | passwd "root" --stdin

# Xfce installation and x2go, NX protocol is faster than VNC more secure
RUN yum install -y epel-release &&\
    yum install -y wget &&\
    yum groups install -y "Xfce" &&\
    yum groups install -y "MATE Desktop" &&\
    yum groups install -y "GNOME Desktop" &&\
    yum install -y x2goserver x2goserver-xsession

RUN sed -i "s/^#Port 22/Port 22/g" /etc/ssh/sshd_config &&\
    sed -i 's/^#PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config &&\
    sed -i 's/^#MaxAuthTries 6/MaxAuthTries 6/g' /etc/ssh/sshd_config &&\
    sed -i 's/^#RSAAuthentication yes/RSAAuthentication yes/g' /etc/ssh/sshd_config &&\
    sed -i 's/^#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config

RUN ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
RUN adduser dockerx
RUN usermod -aG wheel dockerx
RUN echo 'root:ch@ngem3' | chpasswd
RUN echo 'dockerx:ch@ngem3' | chpasswd

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

