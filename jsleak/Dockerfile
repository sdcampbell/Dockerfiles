FROM golang:latest
RUN go install github.com/channyein1337/jsleak@latest
WORKDIR /app
RUN /usr/bin/wget https://raw.githubusercontent.com/mazen160/secrets-patterns-db/refs/heads/master/datasets/trufflehog-v3.yaml
