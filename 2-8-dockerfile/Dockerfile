FROM ubuntu:latest

RUN apt update
RUN apt install -y emacs

COPY .emacs.d/init.el /root/.emacs.d/init.el

CMD date +"%Y/%m/%d %H:%M:%S ( UTC )"