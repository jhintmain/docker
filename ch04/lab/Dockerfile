FROM diamol/golang AS builder

COPY main.go .
RUN go build -o /server
RUN chmod +x /server

# app
FROM diamol/base

CMD ["/web/server"]
ENV USER=sixeyed
EXPOSE 80

WORKDIR web
COPY --from=builder /server .
COPY index.html .