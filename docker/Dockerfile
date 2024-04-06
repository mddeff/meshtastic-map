FROM node:21-alpine3.18
WORKDIR /app
COPY . .
COPY docker/entrypoint.sh entrypoint.sh
RUN npm install
EXPOSE 8080
CMD [ "/app/entrypoint.sh" ]
