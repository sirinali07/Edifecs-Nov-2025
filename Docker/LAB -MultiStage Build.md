# Multi-Stage Docker Build  

Multi-Stage Docker Build allows you to use **multiple** `FROM` **stages in a single Dockerfile**, where the first stage builds the application and the final stage runs it.
This helps us copy **only the required output** into the final image, making it **smaller, faster, and more secure.**

***Why do we use it?***
* To reduce final image size
* To avoid shipping build tools & unnecessary files
* To improve performance in CI/CD and container deployments
* To keep production images clean and lightweight

---

#### Create Project Directory

```bash
cd ~ && mkdir MultiStageBuild-Lab && cd MultiStageBuild-Lab
```
#### Create `package.json` file

This file defines the Node.js project and its dependencies.

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

#### Create `index.js` file

This file contains the Node.js web server code.

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

#### Create Dockerfile (Multi-Stage Build)

The **multi-stage Dockerfile** helps keep the final image small by separating the **build stage** from the **runtime stage**.

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

Verify Files:

```bash
ls
```
Should list `index.js`, `package.json` & `Dockerfile`

#### Build the Docker Image

Run the Docker build command:

```docker
docker build -t node-multistage:v1 .   
```
> `docker build` – builds a Docker image from the Dockerfile.
> `-t` : tag image name & version
> `.` – uses the current directory as the build context.

After the build completes, list images:

```docker
docker images
```
#### Run the Container

Now start a container from the image:

```docker
docker run -d --name multi-container -p 8080:8080 node-multistage:v1
```
> `docker run` – runs a container from an image.
> `-d` – detached mode (runs in background).
> `--name multi-container` – gives the container a friendly name.
> `-p 8080:8080` – maps host port 8080 to container port 8080.
> `node-multistage:v1` – the image name and tag to run.

Verify the running container

```docker
docker ps
```
You should see `multi-container` with `0.0.0.0:8080->8080/tcp`.

####  Test Application

* Open browser
* In the address bar, enter:
```bash
http://<HOST_PUBLIC_IP>:8080
```
> Replace `<HOST_PUBLIC_IP>` with the **actual public IP** of your host VM
> For example, if your VM public IP is 13.16.87.9, then use: http://13.16.87.9:8080

You should see the styled HTML greeting **“Hello From Multi-Stage Docker Build!”** with **current date/time.**

