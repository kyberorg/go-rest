# Prod Dockerfile with optimizations enabled
# Compile stage
ARG GO_VERSION=1.14

FROM golang:${GO_VERSION}-alpine AS builder

RUN apk update && apk add alpine-sdk git && rm -rf /var/cache/apk/*

RUN mkdir -p /api
WORKDIR /api

COPY ../../go.mod .
COPY ../../go.sum .
RUN go mod download

COPY ../.. .
RUN go build -o application main.go

FROM alpine:3.12

RUN mkdir -p /api
WORKDIR /api
COPY --from=builder /api/application .
ADD ../../assets /api/assets

EXPOSE 8080

ENV GIN_MODE=release

ENTRYPOINT ["./application"]