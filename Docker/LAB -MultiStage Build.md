# Multi-Stage Docker Build  

## Step-1 — 
Create Project Directory

```bash
cd ~ && mkdir MultiStageBuild-Lab && cd MultiStageBuild-Lab
```
Create `package.json` file

This file declares the Node.js project & defines start command.
```bash
vi package.json              
```
Add the given content, by pressing `INSERT`
```json
{
  "name": "example-app",
  "version": "1.0.0",
  "description": "A simple Node.js application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```
save the file using `ESCAPE + :wq!`

Create `index.js` file

This is our simple Node web server that returns output.

```bash
vi index.js              
```
Add the given content, by pressing `INSERT`
```js
const port = process.env.PORT || 8080;
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  const currentDate = new Date();
  const dateTime = currentDate.toLocaleString();

  res.send(`
    <div style="font-family: Arial, sans-serif; text-align: center; padding: 50px;">
      <h1 style="color: dodgerblue; font-size: 50px;">
        Hello From Multi-Stage Docker Build!
      </h1>
      <p style="color: gold; font-size: 20px;">
        Current Date and Time: ${dateTime}
      </p>
    </div>
  `);
});

app.get('/readiness', (req, res) => res.send('Ready !!'));

app.get('/liveness', (req, res) => res.send('Live !!'));

app.listen(port, () => console.log(`Example app listening on port ${port}!`));
```
save the file using `ESCAPE + :wq!`

Create Dockerfile

Multi-Stage concept helps keep final image small & production-ready.
```bash
vi Dockerfile
```
Add the given content, by pressing `INSERT`
```Dockerfile
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
```
save the file using `ESCAPE + :wq!`

Verify Files Exist
```bash
ls
```
Should list index.js, package.json & Dockerfile

Build Docker Image
```docker
docker build -t node-multistage:v1 .   
```
> -t : tag image name & version

```docekr
docker images
```
Runs your app inside container mapped to system port.

```docker
docker run -d --name multi-container -p 3000:3000 node-multistage:v1
```
> -d = detached mode
> -p = port mapping HOST:CONTAINER

```docker
docker ps
```

