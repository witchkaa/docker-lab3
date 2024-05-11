FROM golang:latest AS firststage

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 go build -ldflags '-extldflags "-static"' -o /myapp

FROM gcr.io/distroless/base

COPY --from=firststage /myapp /
COPY --from=firststage /app/templates/index.html /templates/

CMD ["/myapp", "serve"]