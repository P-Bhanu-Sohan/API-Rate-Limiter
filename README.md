# Rate Limiter Spring Boot Application

This project demonstrates a simple rate limiting mechanism implemented using Spring Boot and Redis, featuring a sliding window algorithm. It includes a backend API, a basic frontend for interaction, and a producer to simulate traffic.

## Features

*   **Backend (Spring Boot):**
    *   RESTful API endpoint (`/api/ping`) that is rate-limited.
    *   Implements a sliding window rate limiting algorithm.
    *   Uses Redis as the data store for rate limiting counters.
*   **Frontend (HTML/CSS/JavaScript):**
    *   A retro ASCII-themed web interface to interact with the rate limiter.
    *   Buttons to send single requests or a burst of 100 requests.
    *   Served via Nginx within a Docker container.
*   **Producer (Java):**
    *   A standalone Java application to simulate high-volume requests to the rate limiter.
    *   Runs as a Docker container.

## Prerequisites

Before running this application, ensure you have the following installed:

*   **Docker and Docker Compose:** Essential for building and running all services.

## Setup and Running

To get the entire application stack up and running, navigate to the project root directory and use Docker Compose:

```bash
cd /Users/kishorepingali/Desktop/rate-limiter-spring/
docker-compose up --build
```

This command will:
1.  Build the Docker images for the `backend` and `producer` services.
2.  Start the `redis` service.
3.  Start the `backend` service, linked to Redis.
4.  Start the `frontend` service (Nginx serving `index.html`).
5.  Start the `producer` service, which will automatically send requests to the backend.

### Accessing the Application

*   **Frontend:** Open your web browser and navigate to `http://localhost:8081`.
*   **Backend API:** The backend API is accessible internally within the Docker network. If you need to access it directly from your host (e.g., for testing with Postman), it's exposed on `http://localhost:8080`.

### Stopping the Application

To stop all running services and remove their containers, networks, and volumes:

```bash
cd /Users/kishorepingali/Desktop/rate-limiter-spring/
docker-compose down
```

## Rate Limiting Logic

The rate limiting is configured in `backend/src/main/java/com/example/ratelimiter/service/SlidingWindowRateLimiter.java`.

Currently, it is set to:

*   **`maxRequests`**: 50
*   **`windowSeconds`**: 1

This means that a client (identified by IP address) is allowed a maximum of **50 requests within a 1-second window**. If this limit is exceeded, subsequent requests will receive an HTTP 429 (Too Many Requests) response.
