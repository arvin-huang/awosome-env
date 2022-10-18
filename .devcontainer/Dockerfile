
FROM golang:1.19.2 AS golang
LABEL stage=gobuilder

FROM ubuntu:22.04 AS ubuntu

WORKDIR /tmp

# install tools 
RUN cp /etc/apt/sources.list /etc/apt/sources.list.ori &&\
    sed -i "s@http://.*archive.ubuntu.com@http://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list &&\
    sed -i "s@http://.*security.ubuntu.com@http://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
RUN apt update
RUN apt install iputils-ping git net-tools vim wget zsh unzip -y
RUN chsh -s $(which zsh)

# config path
RUN mkdir -p /root/.config


# oh-my-zsh
COPY ./zsh/ohmyzsh-5.8.1.zip /tmp/ohmyzsh.zip
RUN unzip -o /tmp/ohmyzsh.zip && mv /tmp/ohmyzsh-master /root/.config/.oh-my-zsh

# nvm
COPY ./nvm/nvm-0.39.2.zip /tmp/nvm.zip
RUN unzip -o /tmp/nvm.zip && mv /tmp/nvm-master /root/.config/.nvm

COPY ./config/zshrc /root/.config/.zshrc
RUN touch /root/.config/.zshrc; ln -s /root/.config/.zshrc /root/.zshrc; \
    touch /root/.config/.nvm; ln -s /root/.config/.nvm /root/.nvm

# install golang
COPY --from=golang /usr/local/go /usr/local/go
ENV CGO_ENABLED 0
ENV GOPROXY https://goproxy.cn,direct
RUN /usr/local/go/bin/go env -w GO111MODULE=on &&\
    /usr/local/go/bin/go env -w GOBIN=/usr/local/go/bin &&\
    GOPATH=/usr/local/go /usr/local/go/bin/go install github.com/zeromicro/go-zero/tools/goctl@latest &&\
    /usr/local/go/bin/goctl env check -i -f --verbose


VOLUME [ "/usr/local/go","/root/.config","/root/.ssh" ]




