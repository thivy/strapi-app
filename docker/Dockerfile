# Install dependencies only when needed
FROM node:12-alpine AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app
COPY package*.json package-lock*.json ./
RUN npm ci --production

# Rebuild the source code only when needed
FROM node:12-alpine AS builder
WORKDIR /app
COPY . .
COPY --from=deps /app/node_modules /app/node_modules
RUN npm run build 

# Production image, copy all the files and run next
FROM node:12-alpine AS runner
WORKDIR /app

ENV NODE_ENV production

COPY --from=builder /app ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

EXPOSE 1337

CMD ["yarn", "start"]