# FROM node:14.14.0-alpine as builder
# WORKDIR /app
# COPY ./package.json ./
# RUN npm i
# COPY . .
# RUN npm run build

# FROM nginx
# EXPOSE 3000
# COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
# COPY --from=builder /app/build /usr/share/nginx/html


# Stage 1: Build the React app
FROM node:14 as build-stage

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Stage 2: Serve the app with NGINX
FROM nginx:1.21-alpine as production-stage

# Remove default NGINX configurations
RUN rm /etc/nginx/conf.d/default.conf

# Copy the built React app from the previous stage
COPY --from=build-stage /app/build /usr/share/nginx/html

# Copy custom NGINX configuration
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf

# Set NGINX to listen on port 80
EXPOSE 80

# Start NGINX in the foreground
CMD ["nginx", "-g", "daemon off;"]