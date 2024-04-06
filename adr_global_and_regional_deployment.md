# Architecture Decision Log
## Decision: Choosing a Global-Regional Hybrid Cloud Architecture
**Context:**
Our system needs to handle data access, analytics, data interface, and data ingestion across multiple regions. We also need to maintain a "system of record" in a Model store, with changes published as Change Data Capture events to a message broker. Regional services need to listen to these changes and build a copy of the model database for their consumption. Additionally, when a new farm is onboarded, the system needs to identify the deployment region closest to its physical location.
### **Considered Alternatives:**
1.	**Centralized Architecture**: All services are hosted in a single, central location. This approach can simplify management but may result in latency issues for farms in distant regions especially for ingesting telemetry and alerts.
2.	**Fully Distributed Architecture**: All services are hosted in a single, centralized location. While this approach simplifies management, it may lead to latency issues, particularly for farms in remote regions that need to ingest telemetry data and alerts.
3.	**Global-Regional Hybrid Architecture**: A global instance serves _as the host for the Data Access layer and the analytics service_, while regional deployments manage the _data interface and data ingestion_. This approach is designed to strike a balance between the advantages of both centralized and distributed architectures.

### **Decision:**
We have decided to use a Global-Regional Hybrid Architecture.
### **Rationale:**
The decision to use a Global-Regional Hybrid Architecture is based on the following reasons:
1.	**Performance**: By hosting data interface and data ingestion layers in regional deployments, we can reduce latency (for telemetry ingestion) for farms in those regions.
2.	**Data Synchronization**: The global instance maintains the "system of record" and publishes changes to regional services, enabling them to build derived databases. This ensures data consistency across the system, guaranteeing that regional services always have the most recent model (digital twin & configuration) information at their disposal.

3. **Scalability**: This Global-Regional Hybrid Cloud Architecture empowers us to independently scale both regional and global service stacks. This enhances our system's capacity to manage growth effectively.

4. **Availability**: Both regional and global instances are designed with built-in redundancies, ensuring high availability even in the event of failures.

### **Impact:**
Choosing a Global-Regional Hybrid Architecture will require careful planning and coordination to ensure data consistency and manage deployments across multiple regions.

### **Status:**
Decided

### **Consequences:**
We will need to ensure that our chosen cloud provider can support our architectural requirements. Additionally, we will need to implement robust data synchronization mechanisms to maintain data consistency across the system.