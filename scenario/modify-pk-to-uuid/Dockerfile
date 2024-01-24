FROM node:20-alpine as build

WORKDIR /app

COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile

COPY . .

RUN npx prisma generate

RUN yarn build
## Dev
FROM node:20-alpine as dev

WORKDIR /app

## to keep same directory structure between parse server and admin server,
## copy /app/dist to /app/src (which on workdir in ec2 instance)
COPY --from=build /app/dist/ ./dist
COPY --from=build /app/node_modules/ ./node_modules
COPY --from=build /app/prisma/ ./prisma
COPY --from=build /app/package.json ./package.json

CMD ["yarn", "start:dev"]
