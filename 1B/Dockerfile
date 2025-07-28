# Dockerfile
FROM node:18-alpine

WORKDIR /app

# Copy dependency manifests and install
COPY package*.json ./
RUN npm install --production

# Copy all source
COPY . .

# Disable networking inside container
# (will also be passed via `docker run --network none`)

# Default command
CMD ["node", "extract.js"]
