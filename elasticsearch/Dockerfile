FROM elasticsearch:8.15.2
ENV DEBIAN_FRONTEND noninteractive
USER root
RUN apt-get update \
 && apt-get install -y --no-install-recommends build-essential pkg-config locales tzdata \
    vim git curl jq
#    && apt-get clean \
#    && rm -rf /var/lib/apt/lists/*
RUN locale-gen ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV TZ Asia/Tokyo
COPY elasticsearch-8.15.2-analysis-sudachi-3.3.0.zip /usr/share/elasticsearch/
RUN elasticsearch-plugin install file:///usr/share/elasticsearch/elasticsearch-8.15.2-analysis-sudachi-3.3.0.zip
RUN elasticsearch-plugin install --batch analysis-icu
COPY sudachi.json    /usr/share/elasticsearch/plugins/analysis-sudachi/
COPY system_full.dic /usr/share/elasticsearch/plugins/analysis-sudachi/
COPY system_full.dic /usr/share/elasticsearch/config/sudachi/system_core.dic
RUN chown -R elasticsearch:elasticsearch .
USER elasticsearch
