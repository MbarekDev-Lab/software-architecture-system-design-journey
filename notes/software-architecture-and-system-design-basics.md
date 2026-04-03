# Software Architecture and System Design Basics

## Overview

This lecture introduces the core mindset behind software architecture and system design.

The focus is not just on writing code, but on understanding how real systems are structured, how components interact, and how to make design decisions that hold up as systems grow.

This is a foundation lecture for future case studies because it establishes the process used to analyze and design scalable, reliable, and maintainable systems.

## 1. Purpose of the Lecture

The goal of the lecture is to move from theory to practical system thinking.

Instead of focusing only on implementation details, the lecture emphasizes:

- How to break a system into clear parts
- How to reason about scale and failure
- How to evaluate architecture trade-offs
- How to communicate a design in a structured way

## 2. What Software Architecture Means

Software architecture defines the high-level structure of a system.

It describes:

- The main components in the system
- How those components interact
- How the system handles scalability, performance, and reliability
- How the design supports long-term maintainability

### Main Goals of Good Architecture

- Maintainability
- Scalability
- Performance
- Reliability

## 3. System Design Mindset

The lecture promotes a practical design mindset:

- Think in systems, not isolated code files
- Separate concerns clearly
- Design for growth, not only for the current load
- Expect trade-offs instead of perfect solutions
- Consider both normal operation and failure scenarios

This is one of the most important habits for system design interviews and real production work.

## 4. Common System Layers

A well-structured system is usually divided into layers so responsibilities remain clear.

### Client Layer

- Web, mobile, or desktop applications
- Sends requests to backend systems
- Displays data to users

### API Layer

- Exposes endpoints to clients
- Validates requests
- Handles request and response flow

### Business Logic Layer

- Contains application rules and core workflows
- Coordinates operations between services and storage systems

### Data Layer

- Stores persistent data
- Includes databases, object storage, caches, and search systems

### Key Principle

Separate concerns so each layer can evolve, scale, and be maintained more easily.

## 5. High-Level System Components

At the simplest level, many systems include three major parts:

### Client

- Frontend application used by the end user
- Sends requests to the backend

### Server

- Handles business logic
- Processes client requests
- Communicates with databases and other services

### Database

- Stores persistent application data
- Can be relational or NoSQL depending on the use case

### Basic Request Flow

```text
Client -> API Request -> Server -> Database -> Server -> Response -> Client
```

This is the basic end-to-end path that most backend-driven applications follow.

## 6. High-Level Design vs Low-Level Design

### High-Level Design (HLD)

High-Level Design gives a broad view of the system.

It typically includes:

- Major components
- Interactions between components
- External dependencies
- Data flow between services

Example:

```text
Client -> API -> Services -> Database
```

### Low-Level Design (LLD)

Low-Level Design focuses on internal implementation details.

It typically includes:

- Class design
- Database schema
- API contracts
- Algorithms and logic within services

### Main Difference

HLD explains how the system is organized.

LLD explains how each component is implemented.

## 7. Monolithic vs Microservices

### Monolithic Architecture

In a monolith, the application is built as one main codebase and usually deployed as a single unit.

#### Advantages

- Simpler to start with
- Easier initial development and deployment
- Good for smaller systems and early-stage products

#### Drawbacks

- Harder to scale individual parts independently
- Tight coupling makes large systems harder to maintain
- Deployment risk grows as the system becomes larger

### Microservices Architecture

In microservices, the system is split into smaller independent services, each responsible for a specific capability.

#### Advantages

- Better independent scaling
- Independent deployment of services
- Better fault isolation

#### Drawbacks

- More operational complexity
- More complex debugging and observability
- Requires service-to-service communication and coordination

### Main Insight

Microservices are not automatically better. They provide flexibility at the cost of added complexity.

## 8. APIs and Communication Patterns

Systems need clear communication mechanisms between clients and services, and sometimes between services themselves.

### Common API Styles

- REST
- GraphQL
- gRPC

### Communication Models

#### Synchronous Communication

- Request and response happen immediately
- Common for direct client-server workflows

#### Asynchronous Communication

- Work is processed later through queues or events
- Useful for background jobs, retries, and decoupling services

### Why This Matters

The communication style affects latency, reliability, coupling, and scalability.

## 9. Database Design Considerations

Database choice should follow the access patterns and system requirements.

### Relational Databases

- Good for structured data
- Strong consistency and relationships
- Suitable when joins and transactions matter

### NoSQL Databases

- Good for flexible schemas and large-scale distribution
- Useful when scalability and access pattern flexibility matter more than strict relational modeling

### Important Concepts

- Indexing improves query speed
- Normalization reduces duplication
- Denormalization can improve read performance
- Read-heavy and write-heavy systems require different optimizations

## 10. Scalability Concepts

Scalability is the ability of a system to continue performing well as demand increases.

### Vertical Scaling

- Increase CPU, RAM, or disk capacity on a single machine
- Simpler at first
- Eventually reaches hardware limits

### Horizontal Scaling

- Add more machines
- Supports larger scale and better fault tolerance
- Requires distributed system design

### Load Balancing

- Distributes traffic across multiple servers
- Prevents a single machine from becoming overloaded
- Improves availability and operational stability

## 11. Performance Optimization

Performance improvements should target real bottlenecks, not guesses.

### Caching

- Stores frequently accessed data closer to the consumer
- Reduces response time
- Lowers database load

Examples:

- In-memory cache such as Redis
- CDN caching for static content

### Database Optimization

- Proper indexing
- Better query design
- Efficient schema choices

### Asynchronous Processing

- Use queues and background workers for non-immediate tasks
- Helps reduce latency on user-facing requests

## 12. Reliability and Availability

Real systems must continue working even when parts of the system fail.

### Common Techniques

- Replication
- Failover
- Health checks
- Retry mechanisms

### Goal

Keep the system operational and minimize user impact during failures.

## 13. Monitoring and Logging

Production systems need visibility.

Monitoring and logging help teams:

- Track performance
- Detect failures early
- Investigate incidents
- Debug unexpected behavior

Typical examples include:

- Application logs
- Metrics dashboards
- Alerts and health monitoring

## 14. Trade-Offs in System Design

Every architecture decision comes with trade-offs.

Common examples:

| Factor      | Example Trade-Off                          |
| ----------- | ------------------------------------------ |
| Consistency | Strong consistency vs eventual consistency |
| Performance | Speed vs precision or completeness         |
| Cost        | More infrastructure vs higher efficiency   |
| Simplicity  | Faster delivery vs long-term scalability   |

The goal is not to remove trade-offs. The goal is to make them explicit and choose deliberately.

## 15. Practical Case Study Approach

The lecture suggests a repeatable process for solving system design problems.

### Step 1: Clarify Requirements

- Functional requirements: what the system must do
- Non-functional requirements: scale, latency, reliability, durability, cost

### Step 2: Estimate Scale

- Number of users
- Requests per second
- Data size
- Read versus write patterns

### Step 3: Design the High-Level Architecture

- Define major components
- Show how they interact

### Step 4: Deep Dive Into Components

- APIs
- Databases
- Caches
- Queues
- Storage systems

### Step 5: Identify Bottlenecks and Failure Points

- Where will the system slow down?
- What happens when a component fails?

### Step 6: Optimize

- Add caching
- Introduce asynchronous processing
- Scale horizontally where needed

## 16. Example End-to-End Workflow

```text
User sends request from client
        |
        v
Request reaches API gateway or backend server
        |
        v
Business logic processes the request
        |
        v
Data is read from or written to the database
        |
        v
Response is returned to the client
```

This simple flow becomes more complex in real systems, but it is the right starting point for reasoning clearly.

## 17. Key Takeaways

- Start with requirements before choosing technologies
- Think in terms of systems, not just code
- Keep components modular and decoupled
- Design with scale and failure in mind
- Optimize after identifying actual bottlenecks
- Make trade-offs explicit

## 18. Personal Study Notes

Useful habits from this lecture:

- Practice with real case studies such as chat systems, URL shorteners, and file storage systems
- Use diagrams to explain architecture clearly
- Communicate design decisions in a structured order
- Think like both a backend engineer and an infrastructure engineer

## 19. Practice Problems

Good follow-up exercises:

- Design a URL shortener
- Design a messaging system
- Design a file storage system

## Quick Summary

This lecture builds the foundation for practical system design.

The core message is that strong design starts with structured thinking:

- Understand requirements
- Break the system into components
- Reason about scale and reliability
- Identify trade-offs
- Improve the design step by step

That mindset is what makes later case studies easier to understand and explain.
