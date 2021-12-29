FROM node:14

ENV HOME=/app

RUN mkdir -p /app

RUN apt-get update

COPY package.json $HOME

WORKDIR $HOME

RUN npm install && npm cache clean --force

EXPOSE 3000

CMD ["npm", "run", "start:debug"]