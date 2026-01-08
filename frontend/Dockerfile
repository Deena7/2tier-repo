FROM node:18-alpine AS build
WORKDIR /app

# Copy package files
COPY package*.json ./

# Force install ALL dependencies (including devDependencies)
# We use --include=dev to ensure the Angular builder is installed
RUN npm install --include=dev

# Copy the rest of the code
COPY . .

# Run the build
RUN npm run build -- --configuration production

# Stage 2: Serve with Nginx
FROM nginx:alpine
# Check your angular.json for the 'outputPath'. 
# It is likely dist/angular-frontend or just dist/
COPY --from=build /app/dist/angular-frontend /usr/share/nginx/html
EXPOSE 80
