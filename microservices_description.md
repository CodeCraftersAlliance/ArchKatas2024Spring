# Microservices Description

## Overview

In our system architecture, we have implemented several microservices to handle specific functionalities. Each microservice is designed to serve a specific purpose and contribute to the overall system's functionality.

## API Gateway

Here are the reasons why we utilize an API Gateway:

- All APIs exposed to the external world are routed through the API Gateway, providing a single entry point for all external requests.
- The API Gateway integrates with the identity provider and authenticates all incoming requests, ensuring security.
- The API Gateway performs basic parameter validation, ensuring data integrity.
- It maintains an allow or deny list and makes decisions accordingly, providing an additional layer of access control.
- It provides a mechanism for rate limiting to prevent Denial of Service (DoS) attacks, enhancing system resilience.
- The API Gateway allows for the replacement of API routes of services with new ones, without impacting the consumers, ensuring seamless updates.
- It provides a provision to implement circuit breakers in case any of the downstream services are temporarily unavailable, improving system reliability.
- Global error handling can be performed at the API Gateway, centralizing error management.
- Logging and monitoring can be done at a central place through the API Gateway, simplifying system oversight and management.

## Device access and Config layer (deployed globally)

### Gateway Provisioning Service

- Premise: Each manufactured gateway comes preconfigured with authentication credentials and is assigned a specific Gateway Provisioning Service and API endpoint. The Gateway Provisioning Service also updates its enrollment list accordingly.

**Purpose**: 

- When the gateway is powered on for the first time, it connects to the Gateway Provisioning Service endpoint and presents its authentication credentials. 
- The Gateway Provisioning Service verifies the gateway's identity against its enrollment list. 
- Once the gateway's identity is confirmed, the service assigns the gateway device to an IoT hub and registers it in the hub based on the region of the farm to which the gateway is associated.
- The Gateway Provisioning Service receives the unique gateway device ID and registration information from the assigned hub and relays this information back to the gateway device. The gateway then uses its registration information to connect directly to its assigned IoT hub and authenticate itself.
- Following successful authentication, the gateway and IoT hub commence direct communication. The Gateway Provisioning Service's role as an intermediary concludes at this point, unless the gateway requires reprovisioning in the future.

### Onboarding API

**Purpose**: 
- This API provides all the essential endpoints for adding a farm company, farm, enclosure, gateway, sensors, cameras, and users. 

### Configuration API

**Purpose**: 
- This API houses all the necessary endpoints for associating a gateway with a farm, enabling a user to save their notification preferences, setting sensor thresholds for alerts, choosing preferred metrics or details to be displayed in the web interface, selecting a preferred language, and determining the preferred units.

### Insights API

**Purpose**: 
- This API is built on GraphQL (for reasons behind choosing GraphQL, refer to the Architecture Decision Record). Upon successful user login, the API retrieves all necessary details for the farms the user has access to and organizes these farms based on their respective regions. It then initiates synchronous API calls to all regional APIs to gather the required data, such as alarms or telemetry. This data is consolidated and relayed to the caller. However, it's crucial to note that tail-end latencies can potentially impact the performance of this API. Additionally, this API is equipped to fetch insights generated by the advanced analytics service.

### Model Service

**Purpose**: 
- This acts as a facade for the digital twin data stored in the graph database (for reasons behind choosing a Graph database, refer to the Architecture Decision Record). It offers options to create various vertices, edges, and their corresponding attributes, facilitating efficient querying and retrieval of required results.
Additionally, this service publishes change feed or change data capture events to a message broker. This allows other services that rely on digital twin information to build their own copies in the format that best suits their needs. This feature enhances the scalability, flexibility and adaptability of the system, ensuring that all services can effectively utilize the digital twin data. 

### Notification Service

**Purpose**: 
- This service operates as a background service that continuously listens to a message broker, such as an Azure Event Hub.
- Alert processing services, deployed in various regions, publish alerts to this same message broker. Similarly, weather data ingestion services in different regions publish alerts related to inclement weather conditions to the same broker.
- The Global Advanced Analytics service also publishes any alerts based on detected anomalies to this message broker.

The Notification Service processes these alert messages, refers to the model service to enrich the context, and sends notification alerts based on user-configured preferences. This ensures that all relevant alerts are efficiently processed and delivered according to user preferences, providing a comprehensive and personalized alert system. 

### Advanced Analytics Service

**Purpose**: 
- This service includes a data lake house that aggregates all telemetry data from regional deployments, as well as a machine learning (ML) training component. It also maintains a persistent database to store insights for future use.
- The service features an API that utilizes the trained model to predict the yield based on a given set of parameters.
- Furthermore, if the service identifies any anomalies, it promptly sends alerts to the notification service. This ensures immediate awareness and facilitates timely response to potential issues. 

## Data Ingestion Layer

### Telemetry Ingestion Service

**Purpose**: 
- This service processes the stream of telemetry data (coming from IoT hub) related to points and their values, and persists them in the time series database.

### Alert Processing Service

**Purpose**: 
- This service monitors alert messages sent by the gateway as part of the telemetry. It enriches these alerts by referring to its own replica of the digital twin model database, stores them in the time series database, and then publishes them to a message broker (like Azure Event Hub). This allows the alerts to be processed by the global notification service.

### Weather Data Ingestion Service

**Purpose**: 
- This is a continuously running background job that fetches weather information for all locations where farms are situated. The list of farms and their locations is retrieved from its own replica of the digital twin model database. The service then stores this weather data into the time series database.


### Conclusion

These microservices work together to provide a scalable and modular architecture, allowing us to efficiently handle different functionalities within our distributed system.
