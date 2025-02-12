FROM ubuntu:24.10

RUN apt update && \
    apt install -y --no-install-recommends ca-certificates wget gpg && \
    update-ca-certificates

RUN wget -O - https://apt.corretto.aws/corretto.key | gpg --dearmor -o /usr/share/keyrings/corretto-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/corretto-keyring.gpg] https://apt.corretto.aws stable main" | tee /etc/apt/sources.list.d/corretto.list

RUN apt update && \
    apt install -y --no-install-recommends locales java-11-amazon-corretto-jdk && \
    rm -rf /var/lib/apt/lists/* && \
    apt clean && \
    localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

ENV LANG=en_US.UTF-8

RUN mkdir -p ~/.config/aws-sct
RUN wget --no-check-certificate -O ~/.config/aws-sct/AWSSchemaConversionToolBatch.jar https://s3.amazonaws.com/publicsctdownload/jars/AWSSchemaConversionToolBatch.jar
RUN echo 'alias sct="java --add-opens java.base/jdk.internal.loader=ALL-UNNAMED -Djdk.jar.maxSignatureFileSize=20000000 -jar /root/.config/aws-sct/AWSSchemaConversionToolBatch.jar"' >> ~/.bashrc 

WORKDIR /root

CMD [ "/bin/bash" ]
