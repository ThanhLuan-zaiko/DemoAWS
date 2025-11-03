# ./frontend/Dockerfile
FROM node:20-alpine AS build
WORKDIR /app

# copy package.json và lock file nếu có (chỉ copy những file chắc chắn có)
COPY package.json ./

# dùng npm ci nếu có package-lock.json, fallback sang npm install nếu không
RUN if [ -f package-lock.json ]; then npm ci --legacy-peer-deps; else npm install --legacy-peer-deps; fi

# copy toàn bộ source sau khi đã cài dependencies để tận dụng cache
COPY . .
RUN npm run build

FROM nginx:1.27-alpine
COPY --from=build /app/dist /usr/share/nginx/html
RUN rm -f /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/app.conf
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
