Steps to run this project in Local Enviornment or Ec2 Instance

1. Rename the dotenv file to .env file of both frontend folder & backend folder and update these bellow lines as per instructions

MONGO_URI=mongodb account -cluster create-generateusename password(my link is  mongodb+srv://systemmail150:zmsa3fWpBo7Dd021@rabbit.qlgwjlx.mongodb.net/?retryWrites=true&w=majority&appName=rabbit     )     & then give your ec2-instance-public-ip under Add IP address section
JWT_SECRET=    <paste_here>     (use this to generate jwt token:  node -e "console.log(require('crypto').randomBytes(32).toString('hex')"
CLOUDINARY_CLOUD_NAME=XXXXX
CLOUDINARY_API_KEY=XXXXX
CLOUDINARY_API_SECRET=XXXXX

=>(Extra step have to follow):
write this instead in package.json under bellow script section::   "start": "node /backend/server.js",
"scripts": {
    "start": "node server.js",
    "dev": "nodemon backend/server.js",
    "seed": "node seeder.js"
  },


2. Now run these command one by inside backend folder:
npm install
npm run seed
npm run dev -- --host 0.0.0.0


3. VITE_PAYPAL_CLIENT_ID=XXXXX (if you have any or leave it blank)
VITE_BACKEND_URL=http://localhost:9000    OR  ec2-instance-public_ip:3000  (if you have ec2 instance)

4. Now run these command one by one inside frorntend folder:
npm install
npm run dev -- --host 0.0.0.0 





======================================================================================================================





Dockerize this project
=>For frontend section
1. create a docker file inside frontend folder & paste these codes bellow
# --- Build Stage ---
FROM node:18-alpine AS builder
WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install

COPY . .
RUN npm run build   # Uses the "build": "vite build" script

# --- Production Stage ---
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

2. docker build -t frontend-image .
3. docker run -d -p 80:80 frontend-image


=>For Backend Section 
1. create a dockerfile inside backend folder & paste these codes bellow
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

2. docker build -t backend-image .
3. docker run -d -p 3000:3000 backend-image
