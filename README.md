# PMD

## Usage

```
docker run -it -v "$(pwd):/workdir" jamesmoriarty/pmd:6.17.0 pmd -l apex -R rulesets/apex/ruleset.xml -d .
```

## Dockfile

```
FROM openjdk:8-jre-slim

ARG PMD_VERSION=6.18.0

RUN apt update && \
  apt install -y \
    curl \
    unzip \
  && rm -rf /var/cache/apt/*

WORKDIR /opt

RUN curl -sLO https://github.com/pmd/pmd/releases/download/pmd_releases%2F${PMD_VERSION}/pmd-bin-${PMD_VERSION}.zip && \
    unzip pmd-bin-*.zip && \
    rm pmd-bin-*.zip && \
    echo '#!/bin/bash' >> /usr/local/bin/pmd && \
    echo '#!/bin/bash' >> /usr/local/bin/cpd && \
    echo "/opt/pmd-bin-${PMD_VERSION}/bin/run.sh pmd \"$\@\"" >> /usr/local/bin/pmd && \
    echo "/opt/pmd-bin-${PMD_VERSION}/bin/run.sh cpd \"$\@\"" >> /usr/local/bin/cpd && \
    chmod +x /usr/local/bin/pmd && \
    chmod +x /usr/local/bin/cpd

WORKDIR /workdir
```