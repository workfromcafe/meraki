# BASE STAGE
# =============================
FROM node:14-alpine AS base


# =============================
# BUILD STAGE DEVLEOPMENT
# =============================

FROM base AS development

ENV SERVER_PORT=3000
ENV PATH /node/node_modules/.bin:$PATH
EXPOSE $SERVER_PORT 9229

WORKDIR /node
COPY package*.json ./
RUN \ 
    NODE_ENV=development \
    npm ci && \
    npm cache clean --force

WORKDIR /node/app
CMD ["npm", "start"]


# =============================
# SOURCE STAGE 
# =============================

FROM base AS source

WORKDIR /node
COPY package*.json ./
RUN \ 
    npm ci && \
    npm cache clean --force

COPY . .

# =============================
# BUILD TEST STAGE
# =============================

FROM source AS test

ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}
ENV PATH /node/node_modules/.bin:$PATH

COPY --from=development /node/node_modules /node/node_modules

RUN npm run test && npm run lint

# =============================
# BUILD STAGE PRODUCTION
# =============================

FROM source AS production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}
ENV PATH /node/node_modules/.bin:$PATH
ENV SERVER_PORT=3000

EXPOSE $SERVER_PORT
CMD ["npm", "run", "start"]