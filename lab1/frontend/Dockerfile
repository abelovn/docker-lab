FROM node:12-alpine
WORKDIR /usr/src/app
COPY . .
EXPOSE 3000
RUN npm install --silent
RUN npm run build && npm install -g serve
CMD ["serve", "-s", "build"]
