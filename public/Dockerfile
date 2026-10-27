# 1 этап: сборка статики Vue
FROM node:18-alpine AS build-stage
WORKDIR /app

# Копируем только файлы манифеста зависимостей
COPY package*.json ./

# Устанавливаем ВСЕ зависимости (включая devDependencies для сборки)
RUN npm install

# Копируем исходный код (при наличии .dockerignore локальный node_modules не перезапишет установленное)
COPY . .

# Сборка
RUN npm run build

# 2 этап: легковесный веб-сервер Nginx
FROM nginx:stable-alpine AS production-stage

COPY --from=build-stage /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]