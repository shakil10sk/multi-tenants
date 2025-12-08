# Use ARM64 Node image
FROM --platform=linux/amd64 node:20

WORKDIR /app

# Install build tools
RUN apt-get update && apt-get install -y python3 make g++ git curl && rm -rf /var/lib/apt/lists/*

# Copy package.json and package-lock.json first (better cache)
COPY package*.json ./

RUN rm -rf node_modules package-lock.json

# Force install all dependencies including devDependencies
RUN npm install

# Copy the rest of the code
COPY . .

# Use CMD to start Vite dev server
CMD ["npm", "run", "dev"]
