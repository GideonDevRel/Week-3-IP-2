# Use an official Node runtime as a parent image
FROM node:16-alpine AS build

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the package.json and package-lock.json files to the container
COPY package*.json ./

# Install application dependencies
RUN npm ci --only=production

# Copy the rest of the application code to the container
COPY . .

# Production stage
FROM node:16-alpine AS production

WORKDIR /app

# Create a non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodejs -u 1001

# Copy the app from build stage
COPY --from=build --chown=nodejs:nodejs /usr/src/app /app

# Switch to non-root user
USER nodejs

# Expose the port the app runs on
EXPOSE 3000

# Define the command to run your app
CMD ["npm", "start"]