FROM nginx:alpine
COPY index.html /build/index.html
COPY default.conf /etc/nginx/nginx.conf

# Port
EXPOSE 80 443

# defined in package.json
CMD ["nginx", "-g", "daemon off;"]
