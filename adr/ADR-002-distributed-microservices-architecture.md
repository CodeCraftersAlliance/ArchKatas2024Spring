# Chosing architecture style

## Decision
We have decided to use a distributed microservices architecture for our system.

## Status

Accepted

## Context

We need to design a system that can be distributed across multiple regions. The system should be scalable, resilient, and easy to maintain.

### Consequences
- **Pros**:
    - Scalability: Each microservice can be independently scaled based on demand, which is beneficial for our regional deployments that may experience varying loads.
    - Fault Isolation: Failures are isolated to individual services, enhancing the overall uptime of our system.
- **Cons**:
    - Complexity:The introduction of microservices increases complexity in deployment, inter-service communication, and data consistency. This necessitates robust DevOps practices and potentially additional tools.
    - Network Latency: Inter-service communication may experience increased latency due to network overhead. We've designed our system to minimize this by ensuring most services with core business functionality are independent.
    - Data Duplication: The use of individual databases for each microservice may lead to data duplication.
    - Data Consistency: The use of separate databases for different microservices could result in data inconsistencies. We've addressed this by designing our system to effectively handle "Eventual Consistency".

## Alternatives Considered
- Monolithic Architecture: This approach, where a single application handles all tasks, was rejected due to its lack of scalability and potential for single points of failure.


## Rationale
The distributed microservices architecture was selected for its scalability and fault isolation capabilities. Despite the increased complexity, the benefits in terms of scalability and fault isolation outweigh the drawbacks, especially since most of these drawbacks are mitigated in our design.