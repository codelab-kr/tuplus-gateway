FROM node:20.3.1-alpine3.17 as builder
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20.3.1-alpine3.17
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci --omit dev
COPY --from=builder /usr/src/app/dist/ ./dist/
COPY ./src/views/ ./dist/views/
COPY ./public ./public

CMD npx wait-port metadata:80 && \
    npx wait-port history:80 && \
    npx wait-port video-streaming:80 && \
    npm start