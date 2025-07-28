# 1. Use an official Node.js image for AMD64, slim footprint
FROM node:18-alpine

# 2. Create and switch to the app directory
WORKDIR /app

# 3. Copy package manifests and install only production deps
COPY package*.json ./
RUN npm install --production

# 4. Copy the rest of your code
COPY . .

# 5. Default command: process all PDFs under /app/input
#    and write outputs to /app/output
CMD ["node", "extract.js"]
