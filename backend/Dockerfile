FROM node:18-alpine

# Create app directory
WORKDIR /app

# Install all dependencies
COPY package.json package-lock.json ./
RUN npm ci

# Copy the rest of your app code
COPY . .

# Expose the backend port (replace 5000 if your app uses a different one)
EXPOSE 3000

# Optional: Seed the database if needed
# RUN npm run seed

# Start the server in production mode
CMD ["npm", "start"]

