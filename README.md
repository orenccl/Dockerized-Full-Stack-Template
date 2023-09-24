# Dockerized-Full-Stack-Template

Dockerize a full-stack React app with Node.js and MongoDB, supporting both Podman and Docker.

## Environments

- Operating System: CentOS Stream 9
- Node.js: 16.20.1
- Podman: 4.6.1
- URL: /home/test/

## Build images

### Backend image

To build the backend image, use the following command:

```bash
podman build -t backtend backend$ podman build -t backtend backtend
```

### Frontend Development image

To build the frontend development image, use the following command:

```bash
podman build -t frontend frontend
```

### Frontend Production image

To build the frontend production image with version 1.0.0, use the following command:

```bash
podman build -t frontend:1.0.0 -f frontend/Dockerfile.prod frontend
```

## Run the project with separate containers

```bash
podman run -d --name db -p 27017:27017 -v vidly:/data/db docker.io/library/mongo:4.0-xenial

podman run -d --name backend -p 3001:3001 -e DB_URL=mongodb://db/vidly localhost/backend:latest

podman run -d --name frontend -p 3000:3000 localhost/frontend:latest
```

## Run the project with a Pod manually

```bash
podman pod create --name myPod --network podman -p 27017:27017 -p 3001:3001 -p 3000:3000

podman run -d --name db --pod myPod -v vidly:/data/db docker.io/library/mongo:4.0-xenial

podman run -d --name backend --pod myPod -v -e DB_URL=mongodb://db/vidly localhost/backend:latest

podman run -d --name frontend --pod myPod -v localhost/frontend:latest
```

## Development with Podman Kube

In development mode, we bind volumes with our local directory to see changes in real-time. Please follow these steps:

```bash
# Disable SELinux, which is enabled by default in Redhat and CentOS
setenforce 0

# Make sure to npm install or bind volumes will result in loss of modules inside the container
cd frontend
npm install
cd ..

cd backend
npm install
cd ..

# The image will be built automatically, but the container name must match the subdirectory name
podman kube play podman-pod.yml
```

## Test with Podman Kube

```bash
podman kube play podman-pod.test.yml

# Display running processes
podman ps

# Check if every test passes, replace 'xxx' with the container ID
podman logs xxx
```

## Production with Podman Kube

For production, you need to create the production image manually. Ensure the tag matches the YAML settings:

```bash
# We need to create production image manually, tag must match YAML setting
cd frontend
sudo podman build -t frontend:1.0.0 -f frontend/Dockerfile.prod frontend
cd ..

# Port 80 requires superuser permissions
sudo podman kube play podman-pod.prod.yml

```

## Final result
![backend](https://github.com/orenccl/Dockerized-Full-Stack-Template/assets/16741926/f3c3ca60-fedb-4a6b-9c8c-7034ef7a05c9)

![frontend](https://github.com/orenccl/Dockerized-Full-Stack-Template/assets/16741926/16ed177f-d776-4f35-8aff-a3637bd4e676)

## Development and Production image size compare
![size compare](https://github.com/orenccl/Dockerized-Full-Stack-Template/assets/16741926/06dd779b-2761-4780-8058-9ade0fabf835)

