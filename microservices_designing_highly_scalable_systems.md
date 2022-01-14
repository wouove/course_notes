# Microservices: Designing Highly Scalable Systems
https://www.udemy.com/course/introduction-to-microservices/learn/lecture/15301084#overview

## Introduction

* Independence is more important than not duplicating code.
* Micro-services should not share a common db. It will create a single point of failure.
* Deployment should be automated. You have many more services than with a monolith. If deployment is manual, the 
  benefits of the service will be diminished by the downsided of having to deploy so many services.

## Building blocks
* Clients: face the client. Dashboard, app, chatbot
* Container host: hosts containerized micro-services. Each services has its own db
* API Gateway: Single connection between clients and services. Client does not connect micro-service directly, but 
  make calls through the gateway.
* Authentication service: One service to manage authentication to all services. Easy for adding new users and 
  standardizing authentication. Can be done by using tokens.
* Event bus: message queue.

* API Gateway: Can also do load balancing, SSL termination and authentication
* Risk of direct client access without API Gateway:
  * Clients need to keep track of different service endpoints.
  * If the version is updated without a schema change, the client still need to update their URL.
* Authentication: 
  * You can have a dedicated micro-service for authentication and managing users
  * Best practice: Use a one-way hashing algorithm and store only the hashed pw and hash the pw when making a 
    request and compare.
    
## Data management
* Command Query Responsibility Segregation (CQRS) 
  * Command: operation which changes the state of an object or entity
  * Query: operation to retrieve the state of an object or entity
  * Separate API for updating and querying the database
  * Benefits:
    * Data is usually more often read than written
    * Can be optized separately
    * Have two separate dbs for this
* Event-sourcing:
  * Changes to state of entity is stored as sequence of events.
  * Combine CQRS and Event-sourcing:
    * Have a separated events db, can be relational.
    * Updates to state can also be published to a message bus
    * The read db can then be NoSQL, optimized for performance, which is connected to the message bus. It will 
      updated its state once a new event is published.
    * Benefits:
      * Creates audit trail of update events
      * State of object can be retrieved by replaying events
      * Removes need to map relational tables to objects.
      * Event store can feed into multiple databases
      * In case of failure, event store can be used to re-create read-db
* Saga pattern:
  * Pattern to implement transaction that involve two or more micro-services
  * A saga is a sequence of local transactions that is executed in one micro-service, which ends with producing a 
    message to a message queue.
  * Compensating  transactions: when one saga fails, a sequence of compensating transactions is performed to undo 
    changes.
  * Choreography-based saga: Each micro-services starts sequence based on message of previous service.
  * Orchestration-based saga: Orchestrator produces command and consumes response, subsequently, it produces the 
    command for the next service, and so on.
    
## Success factors
* Logging
  * Minimum: Exceptions
  * All requests and response content, together with HTTP status codes
  * If message queue is used, all messages, incl. a sequence number so that different messages of the same saga can 
    be linked.
  * Micro-service response times
  * Login attempts
* Monitoring & Alerting
  * Uptime
  * Avg. response time
  * Resource consumption
  * Success/failure ratio of micro-service
  * Access frequencu of client requests
  * Infrastructure dependencies, e.g. API Gateway, network, etc.
* Documentation
  * API documentation: request and response schemas and purpose. Swagger
  * Design documentation: 1) How service fits in the greater services landscape 2) business logic of micro-servcie
  * Dependencies
  * Network and port-allocations
  
## Deployment and Infrastructure
* Containerization of Micro-services
  * Container orchestration platform:
    * Deployment and provisioning
    * Rescheduling of containers that have failed
    * Scaling and load balancing of container instances
    * Resource allocation between containers
* Tools and Technologies
  * Service discovery: catalog of all services and how to reach them.
  * API Gateways: e.g. Kong built on top of NGINX
  * Logging: e.g. Kibana visualization on top of ElasticSearch db
  