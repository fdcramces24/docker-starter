# Use the official Node.js 20 image
FROM node:20

# Set working directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

ENV NODE_ENV=development

# Install dependencies
RUN npm install

# Copy the rest of the application
COPY . .

# Expose the port your socket server will run on
EXPOSE 3000

# Default command (npm run dev to use nodemon)
CMD ["npm", "run", "dev"]
