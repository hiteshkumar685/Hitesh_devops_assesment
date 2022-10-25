FROM docker:latest

RUN apk update && \
    apk add curl

RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
    mv kubectl /usr/local/bin && \
    chmod +x /usr/local/bin/kubectl
