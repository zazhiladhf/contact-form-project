# Stage 1: Build the Next.js application
FROM node:22.10.0-alpine AS builder

# Working directory
WORKDIR /app

# Copy all files
COPY . .

# Install dependencies and build
RUN npm install && npm run build

# Stage 2: Create the production image
FROM node:22.10.0-alpine

WORKDIR /app

# Create app user and group
RUN addgroup -g 1001 nodejs \
 && adduser -D -u 1001 -G nodejs nextjs

# Copy built files
COPY --from=builder /app/.next/standalone ./ 
COPY --from=builder /app/.next/static ./.next/static

# Change ownership AFTER copy
RUN chown -R nextjs:nodejs . 

# Set environment
ENV NODE_ENV=production
ENV PORT=3000
ENV HOSTNAME=0.0.0.0

USER nextjs

EXPOSE 3000

CMD ["node", "server.js"]
