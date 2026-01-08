# Stage 1: Build
FROM node:18-alpine AS build
WORKDIR /app
# Copy package files first to leverage Docker cache
COPY package*.json ./
RUN npm install
# Copy the rest of the source
COPY . .
# Run the build
RUN npm run build -- --configuration production

# Stage 2: Serve with Nginx
FROM nginx:alpine
# Adjust the path below based on where Angular outputs the build (usually dist/project-name)
COPY --from=build /app/dist/frontend /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
