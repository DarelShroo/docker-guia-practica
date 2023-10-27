# /app /usr /lib
#FROM --platform=linux/amd64 node:19.2-alpine3.16
#FROM --platform=$BUILDPLATFORM node:19.2-alpine3.16

#Dependencias de desarrollo
FROM node:19.2-alpine3.16 AS deps
#cd app
WORKDIR /app
#Dest /app
COPY package.json ./
#Instalar las dependencias
RUN npm install 

#Build y test
FROM node:19.2-alpine3.16 AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run test

#Dependencias de Producción
FROM node:19.2-alpine3.16 AS prod-deps
WORKDIR /app
COPY package.json ./
RUN npm install --prod

#Ejecutar la APP
FROM node:19.2-alpine3.16 AS runner
COPY --from=prod-deps /app/node_modules ./node_modules
COPY app.js ./ 
COPY tasks/ ./tasks
#Comando run de la imagen
CMD ["node", "app.js"]

