FROM nginx:alpine

ARG APP_DIR
ARG USER_NAME
ARG USER_ID
ARG USER_GROUP_NAME
ARG USER_GROUP_ID
ARG USER_GROUP_ID
ARG USER_GROUP_ID

ENV APP_DIR=${APP_DIR:-"/var/www"}
ENV USER_NAME=${USER_NAME:-"www-data"}
ENV USER_ID=${USER_ID:-1000}
ENV USER_GROUP_NAME=${USER_GROUP_NAME:-"www-data"}
ENV USER_GROUP_ID=${USER_GROUP_ID:-1000}

LABEL maintainer="Tarrissarh <tarrissarh@gmail.com>"

RUN apk add --no-cache openssl bash curl

RUN set -x ; \
    addgroup -g $USER_GROUP_ID -S $USER_GROUP_NAME ; \
    adduser -u $USER_ID -D -S -G $USER_GROUP_NAME $USER_NAME && exit 0 ; exit 1

RUN mkdir -p $APP_DIR && chown -R $USER_GROUP_NAME:$USER_NAME $APP_DIR

WORKDIR $APP_DIR

RUN mkdir -p /etc/nginx/hosts
RUN mkdir -p /etc/nginx/ssl
RUN mkdir -p /var/logs/nginx

COPY ./hosts /etc/nginx/hosts
COPY ./ssl /etc/nginx/ssl
COPY ./nginx.conf /etc/nginx/nginx.conf

RUN chown -R $USER_GROUP_NAME:$USER_NAME /var/cache/nginx && \
    chown -R $USER_GROUP_NAME:$USER_NAME /var/logs/nginx && \
    chown -R $USER_GROUP_NAME:$USER_NAME /etc/nginx/nginx.conf && \
    chown -R $USER_GROUP_NAME:$USER_NAME /etc/nginx/conf.d

RUN touch /var/run/nginx.pid && chown -R $USER_GROUP_NAME:$USER_NAME /var/run/nginx.pid

USER $USER_NAME

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]