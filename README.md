<img width="1000" height="506" alt="docker-container-management-illustration" src="https://github.com/user-attachments/assets/44417ae3-41b0-40e3-bbbd-a6cd4333ad95" />

# Dockerising Flask Web Application

## Project Overview

A simple Flask web application containerised using Docker, covering image building, port mapping, environment variables, Docker Compose, and publishing to Docker Hub.

Containerisation is the foundation of modern DevOps workflows. Every microservice deployed to AWS ECS, EKS, or Lambda containers starts as a Dockerfile like this one. Packaging an app with its exact runtime environment so it behaves identically on any machine, eliminating "it works on my machine" issues.

## Problem


Applications often depend on the exact runtime environment they were built in (specific language version, OS libraries, dependencies). Without containerisation, moving an app between machines, or into production, risks it breaking due to environment mismatches.

## Solution



Package the Flask application, its dependencies, and its runtime environment into a single portable Docker image using a Dockerfile. Run it as an isolated container with defined port mappings and environment variables, manage it with Docker Compose, and publish it to Docker Hub for reuse anywhere Docker is installed.

## **Overview**

This project demonstrates

- Dockerfile structure
- Image layers & caching
- Docker commands (build, run, stop, inspect)
- Environment variables in containers
- Port mapping (`p 5000:5000`)
- Optimising images using lightweight base image
- Docker Compose
- Pushing image to Docker Hub

## **Project Code Structure**

---

```
flask-app/
 ├─ app.py
 ├─ requirements.txt
 ├─ Dockerfile
 ├─ .dockerignore
 └─ docker-compose.yml
```

## **Technologies Used**



| Technology | Purpose |
| --- | --- |
| Python 3.11 | Programming language |
| Flask | Web framework |
| Docker | Containerisation |
| Docker Compose | Multi-container management |
| GitHub | Version control |
| Docker Hub | Container image registry |

## Preparation


The project consists of five files:

- `app.py` — the Flask application, exposing a homepage route and a `/health` check route
- `requirements.txt` — lists the Python dependencies (Flask)
- `Dockerfile` — instructions for building the Docker image: base image, working directory, dependency installation, app code, exposed port, and startup command
- `.dockerignore` — excludes unnecessary files (virtual environments, cache files) from the built image
- `docker-compose.yml` — declarative configuration to build and run the container with a single command

## Steps



### 1. Run the app locally without Docker

Create a virtual environment, install dependencies, and run the Flask app directly to confirm it works before containerising it.

<img width="567" height="361" alt="image-10" src="https://github.com/user-attachments/assets/926c6769-661c-4eea-8b71-af3c8e18630d" />

http://localhost:5000 and http://localhost:5000/health

<img width="357" height="81" alt="image-11" src="https://github.com/user-attachments/assets/17617ccc-616b-48fa-a9da-5f10fe5083a6" />

<img width="403" height="100" alt="image-12" src="https://github.com/user-attachments/assets/1d350a3e-3b23-4960-8e35-29f4792ad6d0" />



### 2. Write the Dockerfile

Define a lightweight Python base image, copy in dependencies and application code, expose the app's port, and set the startup command.

<img width="514" height="307" alt="image-13" src="https://github.com/user-attachments/assets/c4baa23c-029e-450b-8893-23ba48d14fdd" />


### 3. Build the Docker image

Build an image from the Dockerfile, tagging it with a name and version.

docker build -t flask-app-image:v1 .

<img width="428" height="55" alt="image-14" src="https://github.com/user-attachments/assets/66af4ff8-c94d-4bad-ab11-4a6cf4c1f80b" />


### 4. Run the container

Start a container from the image, mapping a host port to the container's port so the app is reachable in the browser.

docker run -d -p 5000:5000 --name flask-app-container flask-app-image:v1

<img width="725" height="40" alt="image-15" src="https://github.com/user-attachments/assets/b2729ff4-69d3-4975-9644-4ade95f77558" />


### 5. Use environment variables

Override the app's default message at runtime by passing an environment variable to the container, without rebuilding the image.

```jsx
docker run -d \
-p 5000:5000 \
-e APP_MESSAGE="Hello from inside Docker!" \
--name flask-app-container-env \
flask-app-image:v1
```

### 6. Manage the app with Docker Compose

Define the build and run configuration in a `docker-compose.yml` file, then bring the service up and down with single commands.

<img width="460" height="245" alt="image-16" src="https://github.com/user-attachments/assets/e2f0bbdf-c7f5-47f7-9764-f08337a58985" />


### 7. Push the image to Docker Hub

Tag the image with a Docker Hub username, authenticate using a personal access token, and push the image to a public repository.

<img width="613" height="177" alt="image-17" src="https://github.com/user-attachments/assets/a5422bd1-5911-41fd-b3a0-a4c6ef273ea5" />


## Validation & Testing

#### 1. Verify the container is running

Confirm the container's status, port mapping, and name.

```
docker ps
```

<img width="874" height="83" alt="image-18" src="https://github.com/user-attachments/assets/aa0ae298-9033-4cf7-925c-b505ca1260c5" />


#### 2. Test the application endpoints

Visit the homepage and health check routes in the browser to confirm the app responds correctly.

```
curl http://localhost:5001
curl http://localhost:5001/health
```

#### 4. Verify the image on Docker Hub

Confirm the pushed image appears in the Docker Hub repository with the correct tag.

<img width="713" height="750" alt="image-19" src="https://github.com/user-attachments/assets/4a25148d-a1f9-487a-8886-0df5e8676d1a" />


###
