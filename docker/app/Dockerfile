FROM golang:1.16.2-alpine3.13 as builder

RUN echo -e 'https://mirrors.aliyun.com/alpine/v3.6/main/\nhttps://mirrors.aliyun.com/alpine/v3.6/community/' > /etc/apk/repositories \
    && apk update \
    && apk upgrade \
    && apk --no-cache add ttf-dejavu fontconfig make

RUN mkdir /build
WORKDIR /build

COPY . .

RUN go env -w GO111MODULE=on
RUN go env -w GOPROXY=https://goproxy.cn
RUN GOOS=linux GOARCH=amd64 make build

FROM alpine:3.9

COPY --from=builder /build/docker/app/files /
COPY --from=builder /build/artifacts /app/

WORKDIR /app

RUN  echo -e 'https://mirrors.aliyun.com/alpine/v3.6/main/\nhttps://mirrors.aliyun.com/alpine/v3.6/community/' > /etc/apk/repositories && apk --no-cache add tzdata bash \
    && chmod +x /docker/bin/* \
    && chmod +x bin \
    && rm -rf /tmp/* \
    && rm -rf /var/cache/apk/*

ENTRYPOINT ["/docker/bin/entrypoint.sh"]
