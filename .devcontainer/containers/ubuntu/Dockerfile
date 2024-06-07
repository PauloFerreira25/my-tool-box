FROM ubuntu:24.04

ARG DEV_USER=ubuntu
ARG DOCKER_GID=994


RUN apt-get update \
    && apt-get -y install \
    --no-install-recommends --no-install-suggests \
    git-all \
    telnet \
    iproute2 \
    net-tools \
    iputils-ping \
    openssh-client \
    ca-certificates \
    curl \
    sudo \
    glibc-source \
    groff \
    unzip \
    less \
    pip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* 

RUN adduser $DEV_USER sudo \
    && sudo groupadd --gid $DOCKER_GID docker \
    && adduser $DEV_USER docker 

# Add Docker's official GPG key:
RUN apt-get update \
    && install -m 0755 -d /etc/apt/keyrings \
    && curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc \
    && chmod a+r /etc/apt/keyrings/docker.asc \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null \
    && apt-get update \
    && apt-get  -y install \
    --no-install-recommends --no-install-suggests \
    docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* 

RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && sudo ./aws/install \
    && rm -rf .aws awscliv2.zip 

RUN pip install cfn-flip --break-system-packages
    
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER $DEV_USER
ENV PATH=$PATH:/home/ubuntu/.local/bin

