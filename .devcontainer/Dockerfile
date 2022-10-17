
FROM golang:1.19.2 AS golang
LABEL stage=gobuilder

FROM ubuntu:22.04 AS ubuntu

RUN cp /etc/apt/sources.list /etc/apt/sources.list.ori
RUN sed -i "s@http://.*archive.ubuntu.com@http://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
RUN sed -i "s@http://.*security.ubuntu.com@http://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
RUN apt update
RUN apt upgrade

# install ping 
RUN apt install iputils-ping -y
# install git
RUN apt install git -y
# install net-tools 
RUN apt install net-tools -y
# install vim
RUN apt install vim -y

# install golang
COPY --from=golang /usr/local/go /usr/local/go
ENV CGO_ENABLED 0
ENV GOPROXY https://goproxy.cn,direct
RUN /usr/local/go/bin/go env -w GO111MODULE=on
RUN /usr/local/go/bin/go env -w GOBIN=/usr/local/go/bin_1.19.2

VOLUME [ "/usr/local/go" ]




