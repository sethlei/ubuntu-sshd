FROM ubuntu:latest
LABEL ForkedFrom="Aleksandar Diklic https://github.com/rastasheep"
LABEL HostedAt="https://github.com/sethlei/ubuntu-sshd"

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update

RUN apt-get install -y openssh-server python3.10-venv
RUN mkdir /var/run/sshd

RUN echo 'root:root' |chpasswd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN mkdir /root/.ssh

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22

CMD    ["/usr/sbin/sshd", "-D"]