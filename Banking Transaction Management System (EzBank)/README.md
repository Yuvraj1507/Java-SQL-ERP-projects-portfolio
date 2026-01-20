# Microservices Architecture Showcase

This project demonstrates the implementation of a modern **Microservices Architecture** using a variety of industry-standard technologies. The goal is to showcase scalable, secure, and fault-tolerant services built with **Java**, **Spring Boot**, and **Spring Cloud**, running on **Docker** and **Kubernetes**.


## Key Technologies & Features

- **Microservices Architecture**:  
  A set of independently deployable services, each handling specific business logic, built with **Spring Boot**.

- **Spring Cloud**:  
  Utilizes Spring Cloud components for service discovery (Eureka and Spring Load Balancer), centralized configuration (Spring Cloud Config), and resilient mechanism using Resilience4J.

- **Docker**:  
  All services are containerized using **Docker**, ensuring consistency across development, testing, and production environments.

- **Kubernetes & Helm**:  
  Orchestration of services with **Kubernetes** for automated scaling, self-healing, and load balancing. **Helm** charts simplify deployments and management.

- **Kafka**:  
  **Kafka** is used for asynchronous communication between services, enabling event-driven architecture and decoupled service interactions.

- **RabbitMQ**:  
  **RabbitMQ** is implemented for communication between services like **Accounts Service** and **Notification Service**. When a new account is created, a message is published to the RabbitMQ exchange, and the **Notification Service** listens for this message to send a **welcome SMS** and **welcome email** to the customer. After the email is sent, a confirmation is sent back to the **Accounts Service**, ensuring a smooth communication flow. This setup uses **Spring Cloud Functions** with **RabbitMQ** for reliable, decoupled service interaction.

- **MongoDB**:  
  A NoSQL database used for storing unstructured or semi-structured data with high scalability and performance.

- **Microservices Security**:  
  **Spring Security**, **OAuth2**, and **JWT** are integrated for secure API communication, authentication, and authorization across services.

- **Event-Driven Architecture**:  
  Real-time, event-driven processing is managed by **Kafka** and **RabbitMQ**. The use of these messaging brokers ensures that services remain loosely coupled and communicate asynchronously, improving scalability and fault tolerance. As a result, the system can handle high-throughput and fault isolation effectively, ensuring that services can function independently without direct dependency on one another.

## Additional Features

- **Microservices Resiliency**

  To ensure fault tolerance and system reliability, various resiliency patterns have been implemented across the microservices:

  - **API Gateway Circuit Breaker**: Added circuit breakers at the API Gateway level to ensure that if one service fails, requests to that service are gracefully handled without affecting other services.

  - **HTTP Timeout with Fallback**: Implemented configurable HTTP timeouts in all services to avoid prolonged waits in case of service failures. Each service has a fallback mechanism to handle timeouts and return default responses.

  - **Retry Pattern**: The retry pattern has been applied to the **Accounts Service** to retry failed requests a specified number of times, allowing the system to recover from temporary glitches. This includes retry mechanisms both at the API Gateway level and at the method level. In the Accounts Service, the `/api/consumer/fetchCustomer` endpoint simulates a failure and falls back to a recovery method using retry logic within the method.
  
  - **Rate Limiting with Redis**: In the **Services**, **Redis** is used to implement rate limiting, ensuring that the system can handle high request volumes without overwhelming any particular service. To test this feature is recomended to use adobe benchmark that, in latest version of macOS, is installed by default. Open your terminal and run `ab -n 10 -c 2 -v 3 http://localhost:8090/accounts/api/customer/{mobileNumber} `

  These resiliency measures ensure that the system can continue to function even in the event of temporary failures, providing a seamless experience to users.


- **Observability & Monitoring**

  This project integrates robust observability tools to ensure system high availability (HA) by covering the three pillars of observability:

  - **Loki & Grafana for Logging**: Loki aggregates logs from all services and integrates with Grafana to provide a unified interface for log visualization. Logs are categorized by severity (INFO, ERROR, etc.), making it easier to quickly identify and resolve issues.
  - **Prometheus & Grafana for Metrics**: Prometheus collects key metrics such as request count, latency, error rates, and resource usage from all microservices. These metrics are visualized in Grafana dashboards, enabling real-time monitoring of system health and performance.
  - **OpenTelemetry & Tempo for Tracing**: Distributed tracing with OpenTelemetry tracks requests across services, allowing for detailed insights into their journey through the system. Tempo collects and visualizes these traces in Grafana, helping to pinpoint performance bottlenecks and offering a deeper view into service interactions.

  This observability setup ensures efficient system monitoring, fault detection, and troubleshooting, significantly enhancing system reliability and performance.


- **CI/CD Pipeline**:  
  Automated build, test, and deployment pipelines ensuring smooth and consistent delivery to production.


## Microservices

List of microservices **currently** available on our EzBank. Each microservice is built and containerized using different methods for showcasing various containerization techniques.


### 1. **Accounts Service**
- **Description:** This service simulates the creation of a new bank customer and the setup of their account. Each customer is identified by a unique mobile number, and only one account can be created per mobile number.

### 2. **Card Service**
- **Description:** Similar to the Accounts service, each mobile number can only have one associated card. This service is responsible for managing the creation of bank cards.

### 3. **Loans Service**
- **Description:** This service manages loan offerings and tracks the loans a customer can take within EzBank.
