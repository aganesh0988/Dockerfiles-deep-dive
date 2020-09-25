# Start with a proper base image, for a webserver in this case
FROM nginx:alpine

# Set the directory where the file(s) need to go
WORKDIR /usr/share/nginx/html

# Bring in the website
COPY index.html index.html

# No command is needed because it's already in the base image
