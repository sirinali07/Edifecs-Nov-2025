# Stage 1: Build
FROM node:18 AS builder
WORKDIR /app

# Copy package.json and package-lock.json first to leverage Docker cache
COPY package.json ./

# Install dependencies
RUN npm install --only=production

# Copy the entire source code
COPY . .

# Stage 2: Run (Alpine)
FROM alpine:3
WORKDIR /app

# Install Node.js in Alpine
RUN apk add --no-cache nodejs

# Copy built files from the builder stage
COPY --from=builder /app .

# Expose the application port
EXPOSE 8080

# Run the Node.js application
CMD ["node", "index.js"]
