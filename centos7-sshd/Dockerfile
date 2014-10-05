FROM centos:latest
MAINTAINER Amedeo Salvati

RUN yum update -y
RUN yum install -y openssh-server sudo net-tools

RUN useradd admin -G wheel

# comment change password if you want only key authentication
RUN echo 'admin:secret' | chpasswd
RUN echo '%wheel ALL=(ALL) ALL' >> /etc/sudoers.d/admingroup

RUN mkdir -p /home/admin/.ssh
RUN chown -R admin:admin /home/admin/.ssh; chmod 700 /home/admin/.ssh

# uncomment and write your authorized_keys for user admin
ADD authorized_keys /home/admin/.ssh/

# sshd
RUN mkdir -p /var/run/sshd ; chmod -rx /var/run/sshd

# generate host keys with passphrase blank
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''
RUN ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''

# disable UsePrivilegeSeparation
RUN sed -ri 's/UsePrivilegeSeparation sandbox/#UsePrivilegeSeparation no/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePrivilegeSeparation no/UsePrivilegeSeparation no/g' /etc/ssh/sshd_config
# disable PasswordAuthentication
RUN sed -ri 's/PasswordAuthentication yes/#PasswordAuthentication no/g' /etc/ssh/sshd_config
RUN sed -ri 's/#PasswordAuthentication no/PasswordAuthentication no/g' /etc/ssh/sshd_config

EXPOSE 22
CMD    ["/usr/sbin/sshd", "-D"]

