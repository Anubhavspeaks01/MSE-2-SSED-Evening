# ==== Backend Dockerfile (Multi-stage) ====
FROM node:20-alpine AS builder

WORKDIR /app

# Install production dependencies
COPY package.json ./
RUN npm install --omit=dev

# Copy source code
COPY . ./

# Runtime stage
FROM node:20-alpine

# Install curl for health checks
RUN apk add --no-cache curl

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

COPY --from=builder /app/package.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/src ./src

EXPOSE 3000

USER appuser

CMD ["node", "src/server.js"]
