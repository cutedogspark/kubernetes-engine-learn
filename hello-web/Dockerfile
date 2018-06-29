FROM golang:1.10-alpine
ARG  VERSION=1.0.0
ADD . /go/src/hello-web
WORKDIR /go/src/hello-web
RUN go build -ldflags "-X main.Version=${VERSION}" -o hello-web .

FROM alpine:latest
COPY --from=0 /go/src/hello-web/hello-web .
ENV PORT 8080
CMD ["./hello-web"]
