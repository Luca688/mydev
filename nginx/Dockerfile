FROM nginx:alpine

ENV TZ Asia/Shanghai

RUN apk update && apk add --no-cache tzdata && cp /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && apk del tzdata