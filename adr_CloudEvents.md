# Architecture Decision Log
## **Decision: Adoption of [CloudEvents.io](./Cloudevents-json-format.md)  datamodel for Storing Telemetry Data**
**Context**:
- The system requires a standard and extensible data model that supports versioning and flexibility to efficiently store data in the cloud. 

**Considered Alternatives:**
- Custom data model which is record events is the standard way to go, as we tend to reinvent the properties.
**Decision:**
- We decided to go with [CloudEvents.io](https://cloudevents.io/) data model by Cloud Native Foundation and is available for , where data model and libraries are available to generate and store the Cloud Events.


**Rationale**:
- [CloudEvent] is a very well researched event model brought out by the OpenSource Community and Cloud Native Foundation.
- [CloudEvent](https://cloudevents.io/) Provides variety of libraries to process the Events
- This could be exposed to 3rd party utilities also easily and Interoperability and extensibility is built into the model
- Data Model can be read in the attached link [CloudEvent](./Cloudevents-json-format.md)

**Status**:
Decided