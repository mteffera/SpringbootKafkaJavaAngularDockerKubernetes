# Spring-boot-docker-compose-volume

- This repository contains two primary applications:
  - `demo/demo`: a Spring Boot backend service with REST endpoints, H2/JPA persistence, Kafka publisher/consumer, and Resilience4j circuit breakers.
  - `frontend/employee-ui`: an Angular UI app built with Angular CLI and packaged into a static Node.js `http-server` container.

- Important integration points:
  - Backend REST API: `EmployeeController` exposes `/api/employees` for create and list operations.
  - Kafka topic: `EmployeeEventPublisher` sends to the `employee-created` topic, and `EmployeeEventConsumer` listens on the same topic.
  - Default Kafka bootstrap server is `kafka:9092` in compose and Kubernetes manifests.
  - Backend health endpoint is exposed via Spring Actuator at `/actuator/health`.

- Build / run conventions:
  - Backend build: run `demo/demo/mvnw.cmd clean package` on Windows or `./demo/demo/mvnw clean package` on Linux/macOS.
  - Backend Docker build uses `demo/demo/infra.docker/Dockerfile.backend`.
  - Frontend build: inside `frontend/employee-ui`, run `npm install` then `npm run build`.
  - Frontend Docker build uses `frontend/employee-ui/Dockerfile` and serves built assets from `dist/employee-ui/browser`.
  - Local compose startup: `docker compose -f demo/demo/infra.k8s/docker-compose.yml up --build`.

- Kubernetes deployment notes:
  - Manifests are under `demo/demo/infra.k8s/`.
  - `04-backend.yaml` and `05-frontend.yaml` use `imagePullPolicy: Never`, meaning the images must be available locally in the cluster.
  - The frontend service is exposed via NodePort `30420`.


Technologies I used in this project

1. Java(Version 17) + Spring Boot(Version 3.2.5) for building a clean, modular backend service with REST APIs

2. Apache Kafka for event‑driven communication, producers/consumers, and real‑time processing

3. Zookeeper for Kafka coordination and cluster stability

4. Angular(Version 21) for a responsive, modern frontend UI

5. Docker & Kubernetes deployment to containerize the full stack and orchestrate multi‑service environments

6. Kafdrop for Kafka topic inspection and message visibility

7. H2 Database for lightweight, in‑memory persistence during development

8. Resilience4j for circuit breakers and fault‑tolerant service behavior

9. Distributed, containerized architecture demonstrating real‑world deployment patterns
