FROM nginx:alpine as runtime
COPY proxy/default.conf /etc/nginx/nginx.conf
COPY proxy/ssl.conf /etc/nginx/ssl.conf
# Port
EXPOSE 80 443 8082

# defined in package.json
CMD ["nginx"]