FROM node:21 AS builder
WORKDIR /app

# COPY files which will bring dependency of project
COPY package.json .
Copy pnpm-lock.yaml .

RUN npm i -g pnpm
RUN pnpm install

# COPY all file from the current directory to the /app directory in the container
# NOTE: if you are using . . you need to add .dockerignore file to ignore node_modules
COPY . .

RUN pnpm build


FROM node:current-alpine3.19
WORKDIR /usr/local/app

# COPY all file from the builder .next/standalone
COPY --from=builder /app/.next/standalone .
COPY --from=builder /app/.next/static .next/static
COPY --from=builder /app/public public

CMD ["node", "server.js"]

