########################################################################
# BUILD STAGE 1 - Start with the same image that will be used at runtime
FROM golang:1.10.1 as builder
ARG SSH_PRIVATE_KEY=.ssh/id_rsa

# Make ssh dir
RUN mkdir -p /root/.ssh

# Copy over private key, and set permissions
COPY ${SSH_PRIVATE_KEY} /root/.ssh/id_rsa

# Process
RUN cat /root/.ssh/id_rsa && \
     chmod 400 /root/.ssh/id_rsa && \
# Create known_hosts
    touch /root/.ssh/known_hosts && \
    ssh-keyscan bitbucket.org >> ~/.ssh/known_hosts && \
# Process
    git clone git@bitbucket.org:dzaycevteam/server.git app && \
    cd app && \
    mkdir bin && \
    export GOPATH=$(pwd) && \
    export GOBIN=$GOPATH/bin && \
#    go get . && \
    CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o bin/server main.go

########################################################################
# BUILD STAGE 2 - copy the compiled app dir into a fresh runtime image
FROM alpine:latest as runtime
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /go/app/bin/server server
COPY --from=builder /go/app/client_secret.json client_secret.json
CMD ["./server"]