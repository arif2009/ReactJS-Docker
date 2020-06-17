#### Run a React App in a Docker Container

Here I will show the step by step process to run a react App in a Docker Container

##### Tools & Technology :

---

- [Docker Desktop 2.3.0.3](https://www.docker.com/products/docker-desktop)
- [Windows-10 with powershell 5.1](https://docs.microsoft.com/en-us/powershell/)

##### Create a react app and run it to your local machine :

---

```bash
create-react-app my-app
cd my-app
yatn start
```

If everything ok, it will run successfully in your localhost and you will see something like:

```bash
You can now view my-app in the browser.
Local: http://localhost:3000
On Your Network: http://192.168.0.4:3000
```

##### Add a file named Dockerfile.dev with this instruction

---

```bash
# Specify a base image
FROM node:alpine

WORKDIR '/app'

# Install some depenendencies
COPY package.json .
RUN yarn install
COPY . .

# Default command
CMD ["yarn", "run", "start"]
```

##### Now we need to add this project into docker, create docker image and map a port with localhost and docker

---

```bash
# Add your local project into docker and create a docker image
docker build -f Dockerfile.dev .
```

If everything is ok, you will see something like

> Successfully built dockerImageId

> Ex. Successfully built 9a586e7bfdd7

Now map your local port with docker port to expose app from docker

```bash
docker run -p LocalPort:DockerPort ImageId
# Ex. docker run -p 3000:3000 9a586e7bfdd7
```

If everything ok, you will see something like and you can browse it from browser

```bash
You can now view frontend in the browser.
Local: http://localhost:3000
On Your Network: http://172.17.0.2:3000
```

##### Troubleshooting

---

If you are using `react-scripts` version `3.4.0 +`, you may face an issue with react server startup and port mapping. In that case, you have to add `-it` flag like

`docker run -it -p 3000:3000 9a586e7bfdd7`
