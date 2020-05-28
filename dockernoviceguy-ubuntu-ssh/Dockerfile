FROM ubuntu:latest
LABEL maintainer="dockernoviceguy"

# Atualiza SO, e instala pacotes
RUN apt-get update
RUN apt-get install -y openssh-server vim curl
RUN mkdir /var/run/sshd

RUN echo 'root:root' |chpasswd

# Configura open-ssh server
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN mkdir /root/.ssh

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN echo 'Banner /etc/banner' >> /etc/ssh/sshd_config

COPY etc/banner /etc/

# Cria o usuario 'app'
RUN useradd -ms /bin/bash app
RUN adduser app sudo
RUN echo 'app:app' |chpasswd

# Usa o usuario 'app' para installar nvm
USER app
RUN /bin/bash -l -c "curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash"

# Roda nvm e instala o node 12.x
RUN /bin/bash -l -c ". ~/.nvm/nvm.sh && nvm install 12"

# Volta para 'root'
USER root

# Abre as portas
EXPOSE 22
EXPOSE 3090

CMD    ["/usr/sbin/sshd", "-D"]