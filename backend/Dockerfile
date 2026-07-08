# Stage 1: Build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package.json .
RUN npm install --production && \
    rm -rf ~/.npm

# Stage 2: Production
FROM node:18-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup && \
    apk add --no-cache curl

WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY server.js .

USER appuser
EXPOSE 3001
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD curl -f http://localhost:3001/api/health || exit 1

CMD ["node", "server.js"]
