FROM golang:1.16.6 AS builder


WORKDIR /tmp/go-sample-app

# We want to populate the module cache based on the go.{mod,sum} files.
COPY go.mod .
COPY go.sum .

RUN go mod download

COPY . .

# Unit tests
RUN CGO_ENABLED=0 go test -v

# Build the Go app
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o barista .

FROM scratch
COPY --from=builder /tmp/go-sample-app .
EXPOSE 8080
ENTRYPOINT ["./barista"]