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
*   **Producer (Java):**
    *   A standalone Java application to simulate high-volume requests to the rate limiter.

## Prerequisites

Before running this application, ensure you have the following installed:

*   **Java Development Kit (JDK) 11 or higher:** Required for both the Spring Boot backend and the Java producer.
*   **Maven or Gradle:** For building the Spring Boot backend. (This README assumes Maven for build commands).
*   **Docker and Docker Compose:** To easily set up and run the Redis server.

## Setup and Running

Follow these steps to get the application running:

### 1. Start Redis

The backend uses Redis for rate limiting. You can start a Redis instance using Docker Compose:

```bash
cd /Users/kishorepingali/Desktop/rate-limiter-spring/
docker-compose up -d redis
```

This command will start a Redis container in the background.

### 2. Build and Run the Backend

Navigate to the `backend` directory, build the Spring Boot application, and then run it:

```bash
cd /Users/kishorepingali/Desktop/rate-limiter-spring/backend
./mvnw clean install # Or `gradlew clean build` if using Gradle
java -jar target/rate-limiter-0.0.1-SNAPSHOT.jar # Adjust JAR name if different
```

The backend application will start on `http://localhost:8080`.

### 3. Access the Frontend

Open the `index.html` file in your web browser:

```bash
# From the project root directory
open frontend/index.html
```

You can use the buttons on this page to send requests to the backend and observe the rate limiting in action.

### 4. Run the Producer (Optional)

To simulate a high volume of requests, you can run the `SimulateProducer` application:

```bash
cd /Users/kishorepingali/Desktop/rate-limiter-spring/producer
javac SimulateProducer.java
java SimulateProducer
```

This will send 10 requests to the `/api/ping` endpoint with a 1-second delay between each request. You can modify `SimulateProducer.java` to send more requests or adjust the delay.

## Rate Limiting Logic

The rate limiting is configured in `backend/src/main/java/com/example/ratelimiter/service/SlidingWindowRateLimiter.java`.

Currently, it is set to:

*   **`maxRequests`**: 50
*   **`windowSeconds`**: 1

This means that a client (identified by IP address) is allowed a maximum of **50 requests within a 1-second window**. If this limit is exceeded, subsequent requests will receive an HTTP 429 (Too Many Requests) response.
