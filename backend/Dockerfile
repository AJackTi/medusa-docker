# FROM node:20.8.0-alpine AS deps
# WORKDIR /app/backend

# # copy required files
# COPY package.json .
# COPY yarn.lock .

# # install deps
# RUN yarn install --frozen-lockfile

# # intermediate build stage
# FROM node:20.8.0-alpine AS builder
# WORKDIR /app/backend

# # copy node_modules from deps
# COPY --from=deps /app/backend/node_modules /app/backend/node_modules

# # install python, medusa-cli and npx
# RUN apk update
# RUN apk add python3
# RUN yarn global add @medusajs/medusa-cli@latest
# RUN yarn global add npx

# # copy project files to current workdir
# COPY . .

# # run medusa develop
# ENTRYPOINT ["/bin/sh", "./develop.sh", "develop"]


FROM node:16-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

FROM node:16-alpine

WORKDIR /app

RUN npm install -g @medusajs/medusa-cli

COPY --from=build /app .

CMD medusa migrations run && npm run start
