# Project: Run an express app in a Docker container

# Step 1: Use the 'node' official image (a recent version with an alpine base)
FROM node:latest

# Step 2: Look in the app.js to determine which port to expose
EXPOSE 8081

# Step 3: Use alpine package manager to install tini: 'apk add --update tini'
#   (tini is a package that can be used as an entry point in a container)
RUN apk add --update tini

# Step 4: With a single command, create a new directory for app files at the
#   path '/usr/src/app' and make that your current directory
WORKDIR /usr/src/app

# In the next steps, you will load and install packages before applications
#   files for better caching

# Step 5: Since node uses a "package manager", copy in 'package.json' and the
#   corresponding lock file
COPY package.json /usr/src/app/
COPY package-lock.json /usr/src/app/

# Step 6: Install dependencies using npm
#   To keep the image clean and small, run 'npm cache clean --force' after
RUN npm install
RUN npm cache clean --force

# Step 7: Copy in all files from current directory
COPY routes.js /usr/src/app/
COPY app.js /usr/src/app/
COPY views /usr/src/app/

# Step 8. Run the application with command '/sbin/tini -- node app.js'.
#   Remember to use the "exec form" which is the preferred syntax
#   Here is the documentation for CMD if you need a reminder:
#   https://docs.docker.com/engine/reference/builder/#cmd
CMD ["/sbin/tini", "--", "node", "app.js"]