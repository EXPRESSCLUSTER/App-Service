FROM node:latest

RUN mkdir /opt/cwo
WORKDIR /opt/cwo

COPY cluster-webui-lin-4.2.1-200401-1.tgz .

RUN npm install --no-save cluster-webui-lin-4.2.1-200401-1.tgz

EXPOSE 29003
CMD ["/bin/bash","-c","cd /opt/cwo && /usr/local/bin/npx cluster-webui-lin --locale=ja"]
