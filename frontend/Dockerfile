# Stage 1: Build Angular app
FROM node:18-alpine AS builder

WORKDIR /app

COPY package.json .
RUN npm install && npm install @types/node@18.19.0 --save-dev --legacy-peer-deps

COPY . .
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine

COPY --from=builder /app/dist/angular-15-crud /usr/share/nginx/html
COPY nginx-frontend.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
