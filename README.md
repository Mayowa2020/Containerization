# Weather Application - Containerization and Container Orchestration

## Project Overview

This project involves developing a microservices-based weather application. The implementation includes creating two microservices: one for fetching weather data and another for displaying it. The primary objectives are to containerize these microservices using Docker, deploy them to a Kubernetes cluster, and access them through Nginx.

## Table of Contents

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
  - [Phase 1: Basic Frontend Application with Docker and Kubernetes](#phase-1-basic-frontend-application-with-docker-and-kubernetes)
    - [Task 1: Set up your project](#task-1-set-up-your-project)
    - [Task 2: Initialize Git](#task-2-initialize-git)
    - [Task 3: Git Commit](#task-3-git-commit)
    - [Task 4: Dockerize the application](#task-4-dockerize-the-application)
    - [Task 5: Push to Docker Hub](#task-5-push-to-docker-hub)
    - [Task 6: Set up a Minikube Kubernetes Cluster](#task-6-set-up-a-minikube-kubernetes-cluster)
    - [Task 7: Deploy to Kubernetes](#task-7-deploy-to-kubernetes)
    - [Task 8: Create a Service (ClusterIP)](#task-8-create-a-service-clusterip)
    - [Task 9: Access the Application](#task-9-access-the-application)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

- Docker
- Docker Hub account
- Git
- Minikube
- kubectl (Kubernetes command-line tool)

## Project Structure

```
weather-app/
├── index.html
├── styles.css
├── Dockerfile
├── deployment.yaml
└── service.yaml
```

## Setup Instructions

### Phase 1: Basic Frontend Application with Docker and Kubernetes

#### Task 1: Set up your project

1. Create a new project directory:

    ```bash
    mkdir weather-app
    cd weather-app
    ```

2. Inside the directory, create an HTML file (`index.html`) and a CSS file (`styles.css`):

    ```html
    <!-- index.html -->
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Weather App</title>
        <link rel="stylesheet" href="styles.css">
    </head>
    <body>
        <h1>Welcome to the Weather App</h1>
        <p>Stay updated with the latest weather information.</p>
    </body>
    </html>
    ```

    ```css
    /* styles.css */
    body {
        font-family: Arial, sans-serif;
        text-align: center;
        margin-top: 50px;
    }
    h1 {
        color: #333;
    }
    p {
        color: #666;
    }
    ```

#### Task 2: Initialize Git

1. Initialize a Git repository in your project directory:

    ```bash
    git init
    ```

#### Task 3: Git Commit

1. Add and commit your initial code to the Git repository:

    ```bash
    git add .
    git commit -m "Initial commit"
    ```

#### Task 4: Dockerize the application

1. Create a `Dockerfile` specifying Nginx as the base image:

    ```Dockerfile
    # Dockerfile
    FROM nginx:alpine
    COPY . /usr/share/nginx/html
    ```

2. Build the Docker image:

    ```bash
    docker build -t weather-app .
    ```

#### Task 5: Push to Docker Hub

1. Log in to Docker Hub:

    ```bash
    docker login
    ```

2. Tag your Docker image:

    ```bash
    docker tag weather-app your-dockerhub-username/weather-app
    ```

3. Push your Docker image to Docker Hub:

    ```bash
    docker push your-dockerhub-username/weather-app
    ```

#### Task 6: Set up a Minikube Kubernetes Cluster

1. Install Minikube:

    ```bash
    curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    chmod +x minikube
    sudo mv minikube /usr/local/bin/
    ```

2. Start Minikube:

    ```bash
    minikube start
    ```

#### Task 7: Deploy to Kubernetes

1. Create a Kubernetes Deployment YAML file (`deployment.yaml`) specifying the image and desired replicas:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: weather-app
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: weather-app
      template:
        metadata:
          labels:
            app: weather-app
        spec:
          containers:
          - name: weather-app
            image: your-dockerhub-username/weather-app
            ports:
            - containerPort: 80
    ```

2. Apply the deployment to your cluster:

    ```bash
    kubectl apply -f deployment.yaml
    ```

#### Task 8: Create a Service (ClusterIP)

1. Create a Kubernetes Service YAML file (`service.yaml`) specifying the type as ClusterIP:

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: weather-app-service
    spec:
      selector:
        app: weather-app
      ports:
        - protocol: TCP
          port: 80
          targetPort: 80
      type: ClusterIP
    ```

2. Apply the service to your cluster:

    ```bash
    kubectl apply -f service.yaml
    ```

#### Task 9: Access the Application

1. Enable Minikube's ingress addon:

    ```bash
    minikube addons enable ingress
    ```

2. Port-forward to the service to access the application locally:

    ```bash
    kubectl port-forward service/weather-app-service 8080:80
    ```

3. Open your browser and visit `http://localhost:8080` to view your simple frontend application.

## Contributing

Contributions are welcome! Please fork this repository and submit a pull request for any enhancements or bug fixes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.


