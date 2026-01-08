# -------- Build Stage --------
FROM node:18-alpine AS build

WORKDIR /app

COPY package.json angular.json tsconfig*.json ./
RUN npm install

COPY src ./src
RUN npm run build -- --configuration production

# -------- Runtime Stage --------
FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

