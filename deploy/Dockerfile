FROM debian:stretch-slim

RUN apt-get update && \
    apt-get install -y \
        openssh-client \
        wget \
        curl \
        lftp \
        rsync

RUN apt-get install -y \
    python3 \
    python3-pip

COPY deployer/deployer /usr/local/bin/deployer
COPY deployer/requirements.txt .

RUN pip3 install -r requirements.txt
RUN rm requirements.txt

