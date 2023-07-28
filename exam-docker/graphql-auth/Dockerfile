FROM node:20 AS builder

WORKDIR /app

COPY ["package.json", "pnpm-lock.yaml", "/app/"]

RUN npm install -g pnpm
RUN pnpm install
RUN pnpm install prisma

COPY . .

RUN pnpm prisma generate
RUN pnpm prisma migrate dev

RUN pnpm build 

FROM node:20

WORKDIR /app

COPY --from=builder /app/package*.json ./
COPY --from=builder /app/dist ./dist

ENV NODE_ENV=production

RUN pnpm install --only=production

ENTRYPOINT ["node", "./dist/index.js"]