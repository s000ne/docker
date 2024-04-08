FROM nvidia/cuda:12.1.0-base-ubuntu22.04

RUN apt-get update && apt-get install -y ca-certificates curl wget
RUN install -m 0755 -d /etc/apt/keyrings
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc \
    && chmod a+r /etc/apt/keyrings/docker.asc

RUN echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt-get update && apt-get install -y  \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin \
    tini

RUN distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \ 
    && curl -s -L https://nvidia.github.io/libnvidia-container/gpgkey | apt-key add - \
    && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list \
    | tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
    && apt-get update && apt-get install -y nvidia-container-toolkit

# TODO: これはいらないかも
# https://github.com/docker/docker/tree/master/hack/dind
ENV DIND_COMMIT 65cfcc28ab37cb75e1560e4b4738719c07c6618e
RUN set -eux; \
    wget -O /usr/local/bin/dind "https://raw.githubusercontent.com/docker/docker/${DIND_COMMIT}/hack/dind"; \
    chmod +x /usr/local/bin/dind

COPY ./docker-entrypoint.sh /usr/local/bin/

# TODO: これもいらないかも
VOLUME /var/lib/docker
EXPOSE 2375 2376

CMD ["docker-entrypoint.sh"]