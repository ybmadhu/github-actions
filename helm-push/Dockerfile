FROM alpine/helm:3.1.2

RUN apk add --no-cache bash git

RUN helm plugin install https://github.com/hypnoglow/helm-s3.git --version 0.10.0

COPY entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
