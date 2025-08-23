# Stage 0, "build-stage", based on Node.js, to build and compile the frontend
FROM node:20 AS build-stage

# Install git for cloning
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

# Clone the FastAPI template repository
RUN git clone https://github.com/fastapi/full-stack-fastapi-template.git /tmp/template

WORKDIR /app

# Copy package files and install dependencies
RUN cp /tmp/template/frontend/package*.json /app/ && \
    npm install

# Copy all frontend source files
RUN cp -r /tmp/template/frontend/* /app/

ARG VITE_API_URL

RUN npm run build

# Stage 1, based on Nginx, to have only the compiled app, ready for production with Nginx
FROM nginx:1

COPY --from=build-stage /app/dist/ /usr/share/nginx/html

# Copy nginx configuration files
COPY --from=build-stage /app/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build-stage /app/nginx-backend-not-found.conf /etc/nginx/extra-conf.d/backend-not-found.conf