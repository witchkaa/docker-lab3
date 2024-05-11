FROM golang:latest AS firststage

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 go build -ldflags '-extldflags "-static"' -o /myapp

FROM scratch

COPY --from=firststage /myapp /myapp
COPY --from=firststage /app/templates/index.html /templates/

COPY --from=firststage /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

COPY --from=firststage /etc/passwd /etc/passwd

USER nobody

EXPOSE 8080

CMD ["/myapp", "serve"]