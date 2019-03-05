########################################################################
# BUILD STAGE 1
FROM mhart/alpine-node:8 as builder

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
# Add bitbuckets key
    apk add --no-cache git openssh-client && \
    ssh-keyscan bitbucket.org >> ~/.ssh/known_hosts

# Process
RUN git clone git@bitbucket.org:dzaycevteam/web.git app && \
    cd app && \
    yarn install

WORKDIR /app

# Build application  --production
RUN yarn run build


########################################################################
# BUILD STAGE 2
FROM nginx:alpine as runtime
COPY --from=builder /app/build /build
COPY web/default.conf /etc/nginx/nginx.conf

# defined in package.json
CMD ["nginx"]


