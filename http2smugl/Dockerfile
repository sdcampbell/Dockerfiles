FROM golang:1.19.13-bullseye
RUN go install github.com/neex/http2smugl@latest
ENTRYPOINT ["/go/bin/http2smugl"]
