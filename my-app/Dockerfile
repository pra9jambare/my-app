# Use Node.js 18
FROM node:18

# Create app directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies (with verbose for debugging)
RUN npm install --verbose

# Copy all app files
COPY . .

# Expose port
EXPOSE 3000

# Start the app
CMD ["node", "app.js"]
