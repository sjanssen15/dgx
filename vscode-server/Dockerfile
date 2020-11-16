# Ubuntu 18.04
# Will listen on any network
# More info on paramters, run: code-server --help
FROM ubuntu:bionic
RUN apt update -y
RUN apt upgrade -y
RUN apt install git -y
RUN apt install curl -y
RUN curl -fsSL https://code-server.dev/install.sh | sh
CMD code-server --bind-addr "0.0.0.0:9099" --auth "none"