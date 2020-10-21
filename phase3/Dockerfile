# Step 1: Base image for node build - use alpine for node version 14
#         Name it build-stage
FROM node:14-alpine3.12 as build-stage
# Step 2: Set the working directory to /app
WORKDIR /app
# Step 3: Add package files
COPY package*.json ./
# Step 4: Install dependencies using npm
#   To keep the image small, (force) clean the npm cache after
#   Chain the commands to reduce the number of layers in the image
RUN npm install \
    && npm cache clean --force
# Step 5: Add application files
COPY . .
# Step 6: Build application
RUN npm run build
# Step 7: Bring in the base image for NGINX (alpine)
FROM nginx:alpine as webserver
# Step 8: Expose the proper nginx port for http
EXPOSE 80
# Step 9: Set working directory to the html folder for nginx
WORKDIR /usr/share/nginx/html
# Step 10: Copy over the build files from build-stage
COPY --from=build-stage /app/build .
# Step 11: Replace the default NGINX config with the application's version
COPY --from=build-stage /app/nginx.conf /etc/nginx/conf.d/default.conf
