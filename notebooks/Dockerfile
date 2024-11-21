FROM ubuntu:22.04

ENV DEBIAN_FRONTEND noninteractive
ENV VPN 1

WORKDIR /root

RUN apt-get update \
 && apt-get install -y --no-install-recommends build-essential pkg-config locales tzdata \
    python3 python3-dev python3-pip python3-wheel python3-venv pipx \
    vim git curl jq \
    mysql-client \
    libmysqlclient-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN pipx ensurepath

RUN locale-gen ja_JP.UTF-8

ENV LANG ja_JP.UTF-8
ENV TZ Asia/Tokyo
ENV PATH="${PATH}:/root/.local/bin"

RUN ln -sf /usr/bin/python3.10 /usr/bin/python3
RUN ln -s /usr/bin/python3 /usr/bin/python

RUN pipx install poetry

# R
RUN apt-get update -qq
RUN apt-get install -y --no-install-recommends software-properties-common dirmngr
RUN apt-get install -y wget
RUN wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
RUN add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
RUN apt-get install -y --no-install-recommends r-base
RUN apt-get install -y --no-install-recommends gpg-agent
RUN add-apt-repository ppa:c2d4u.team/c2d4u4.0+
RUN apt-get install -y --no-install-recommends r-cran-rstan r-cran-tidyverse r-cran-devtools r-cran-ggpubr r-cran-ggmcmc r-cran-gtools r-cran-irkernel

# required for rpy2
RUN apt-get install -y --no-install-recommends libpcre2-dev libbz2-dev liblzma-dev libicu-dev libblas-dev libdeflate-dev

RUN apt-get install -y fonts-ipafont fonts-ipaexfont
RUN fc-cache -fv

COPY ./src /notebooks

WORKDIR /notebooks
RUN poetry install

#CMD ["poetry", "run", "jupyter", "lab", "--allow-root", "--ip=0.0.0.0", "--NotebookApp.token=''", "--no-browser"]
CMD ["bash"]
