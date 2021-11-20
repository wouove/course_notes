# Introduction

* Independence is more important than not duplicating code.
* Micro-services should not share a common db. It will create a single point of failure.
* Deployment should be automated. You have many more services than with a monolith. If deployment is manual, the 
  benefits of the service will be diminished by the downsided of having to deploy so many services.

# Building blocks
* Clients: face the client. Dashboard, app, chatbot
* Container host: hosts containerized micro-services. Each services has its own db
* API Gateway: Single connection between clients and services. Client does not connect micro-service directly, but 
  make calls through the gateway.
* Authentication service: One service to manage authentication to all services. Easy for adding new users and 
  standardizing authentication. Can be done by using tokens.
* Event bus: message queue.