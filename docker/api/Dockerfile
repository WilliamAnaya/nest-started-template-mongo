FROM node:16-alpine AS BUILD_IMAGE

RUN apk update && apk add --no-cache curl bash && rm -rf /var/cache/apk/*

ARG APP_DIR=/application

WORKDIR $APP_DIR

COPY package.json yarn.lock ./

# install dependencies
RUN npm install

COPY . .

#build application
RUN yarn build

FROM node:16-alpine

WORKDIR /application

COPY package.json ./

COPY --from=BUILD_IMAGE /application/dist ./dist
COPY --from=BUILD_IMAGE /application/node_modules ./node_modules

EXPOSE 3000

ENTRYPOINT ["yarn", "start:dev"]
