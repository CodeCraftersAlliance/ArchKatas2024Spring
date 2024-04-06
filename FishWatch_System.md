- [Welcome to Code Crafters Alliance](#welcome-to-codecrafters-alliance)
  - [1. The Problem](#1-the-problem)
    - [1.1 Functional Requirements](#11-functional-requirements)
      - [Data Ingestion/Acquisition](#data-ingestionacquisition)
      - [Data Consolidation and Display](#data-consolidation-and-display)
      - [Data Recording and Storage](#data-recording-and-storage)
      - [Data Analysis and Alerting](#data-analysis-and-alerting)
      - [User Interface and Interaction](#user-interface-and-interaction)
      - [Prediction Model](#prediction-model)
  - [1.2 Additional Requirements](#12-additional-requirements)
    - [Security](#security)
      - [Scalability](#scalability)
  - [1.3 Users and Roles](#13-users-and-roles)
        - [Table: Users and Roles](#table-users-and-roles)
  - [1.4 Constraints and Assumptions](#14-constraints-and-assumptions)
  - [2. Solution](#2-solution)
    - [3.1 User Personas](#31-user-personas)
      - [John Doe, the farmer](#john-doe-the-farmer)
      - [Charles Xavier, the Customer](#charles-xavier-the-customer)
      - [Natasha Romanoff, the Customer Administrator](#natasha-romanoff-the-customer-administrator)
      - [Nick Fury, the Integrator](#nick-fury-the-integrator)
      - [Tony Stark, the System Administrator](#tony-stark-the-system-administrator)
  - [3.2 Usage Patterns](#32-usage-patterns)
    - [Need for Admin Screens](#need-for-admin-screens)
      - [*FishWatch Monitor Screen*](#fishwatch-monitor-screen)
      - [*Notification History/Alerts History*](#notification-historyalerts-history)
      - [*Sensor Trends*](#sensor-trends)
      - [*Configure Rules*](#configure-rules)
      - [*Manage Enclosures*](#manage-enclosures)
      - [Gateway Hub](#gateway-hub)
    - [*3.3 Architecture Characteristics*](#33-architecture-characteristics)
      - [*Availability*](#availability)
      - [*Data Integrity*](#data-integrity)
      - [*Data Consistency*](#data-consistency)
      - [*Fault Tolerance*](#fault-tolerance)
      - [Concurrency](#concurrency)
      - [Performance](#performance)
    - [3.4 Architecture Style](#34-architecture-style)
  - [**4 System Architecture : Components**](#4-system-architecture--components)
    - [**4.1 System Architecture : Components**](#41-system-architecture--components)
      - [Context](#context)
      - [Component](#component)
    - [**4.2 System Architecture : Dataflow**](#42-system-architecture--dataflow)
  - [5. Detailed Architecture](#5-detailed-architecture)
    - [**5.1 Fish watch Data Model**](#51-fish-watch-data-model)
    - [**5.2 Microservice Independence**](#52-microservice-independence)
    - [5.3 Microservice Descriptions](#53-microservice-descriptions)
      - [**Overview**](#overview)
      - [**API Gateway**](#api-gateway)
      - [**Device access and Config layer (deployed globally)**](#device-access-and-config-layer-deployed-globally)
        - [**Gateway Provisioning Service**](#gateway-provisioning-service)
        - [Onboarding API](#onboarding-api)
        - [Configuration API](#configuration-api)
        - [Insights API](#insights-api)
        - [Model Service](#model-service)
        - [Notification Service](#notification-service)
        - [Advanced Analytics Service](#advanced-analytics-service)
      - [Data Ingestion Layer](#data-ingestion-layer)
        - [Telemetry Ingestion Service](#telemetry-ingestion-service)
        - [Alert Processing Service](#alert-processing-service)
        - [Weather Data Ingestion Service](#weather-data-ingestion-service)
    - [Conclusion](#conclusion)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Welcome to Code Crafters Alliance

Repository for Code Crafters Alliance solution to O'Reilly 2024 Spring Architectural Katas Challenge.

## 1. The Problem

### 1.1 Functional Requirements

From the problem statement, we extracted the following core requirements to guide our proposed architecture for the FishWatch system.

#### Data Ingestion/Acquisition

The data ingestion process involves reading data from various sensors and sources, such as water temperature, pH, dissolved oxygen, ammonia, turbidity, camera feeds, and weather information. This data should be collected at regular intervals, such as every 5 minutes for sensor data and hourly for weather information. The system should be able to handle this data and store it for further processing and analysis.

#### Data Consolidation and Display

The system should consolidate the data from multiple sources ( Enclosures) into a single stream. It should display data on a consolidated monitoring screen at Farmers work stations ( Rugged Tablets/MobilePhones/PCs). Each Enclosures health signs should be displayed on request,

#### Data Recording and Storage

Fishwatch must record and store detail data at least for one harvest cycle . Aggregated data can be stored and be available for view for Customers to review enclosure health and, filtering on time range and other Health parameters.

#### Data Analysis and Alerting

The system should analyze each enclosures signs for abnormalities or preset thresholds. It should alert farmers if there is a consistent variation in PH , Salinity or temperature and the alerts should be timely and configurable to meet the needs of the customer

#### User Interface and Interaction

Fishwatch should provide an intuitive user interface for Farmers and Customers to interact with the system. It should allow the farmers and farm owner(Customer) to configure alerts, view alerts history and various other health parameters

#### Prediction Model  

FishWatch shall be able to provide predictive alerts/recommendations based on the available telemetry as well as external factors like Local weather to take appropriate actions.

## 1.2 Additional Requirements

### Security  

FishWatch should meet high data security and privacy standards in the future. It should ensure user authentication and authorization to maintain data security and privacy. It should ensure secure data exchange between different components. In this current iteration, Fishwatch shall comply with Authentication and Authorization , not adhering to safe harboring in the current scope.

#### Scalability

The system should be scalable to accommodate increasing number of Enclosure and vital sign monitoring devices. The system should be capable of handling peak loads.

- _Integration Capabilities_:   Provide APIs for further dashboarding / customization as per the Customer needs.
- _Fault Tolerance and High Availability_: FishWatch should remain operational even if individual sensors /components fail and duly report /maintain the operability status  to the system. It should implement high-availability mechanisms to minimize downtime and ensure continuous monitoring and alerting services.

## 1.3 Users and Roles

##### Table: Users and Roles

|User Role |Actions|
|----------|-------|
| Livestock® Integrator | - Add Enclosure <br>- Setup Customer Admin<br>- Setup Sensors<br>- Install App to Customer/Farmers<br>- Test run<br>- Setup notification<br>|
|Customer/Customer Admin| - Setup Farm Users<br>- View Enclosure Health<br>-Setup Notifications<br>-Allocate resources(Enclosure) for Farm users<br>-Acknowledge Alerts/notification<br>- Customize Dashboard<br>-View Dashboard|
|Farmer(Farm user) |- Acknowledge Alerts/notification<br>- Setup notification<br>-View Enclosure Health<br>-View Dashboard<br>- Customize Dashboard<br>
|Livestock® Admin |- Review hardware status<br>- Update software version<br>- Notify Firmware updates<br>- Monitor and maintain Database<br>- Review uptime

## 1.4 Constraints and Assumptions

- We have developed a fault-tolerant and high-availability architecture. We have considered The Databases to be PaaS service with redundancy mechanisms, providing failover capabilities to maintain service availability in case of component failures, and business continuity strategies using response and recovery are necessary.
- Our architecture has put data collection at enclosure level as priority, considering fluctuating cellular networks and internet availability. We present a hybrid architecture with on and off connectivity to cloud. We have assumed that the farmers shall be provided with rugged devices/mobile devices which shall have Wifi Connectivity to connect to the enclosure Edge Hubs on Access Points points to access real time data.
- Although we have briefly discussed system Administrator and Integrator, it is not the central topic in our architecture. System administration includes reviewing software and hardware health, performing updates, and backing up data. We assume that Livestock Inc shall be having trained professionals, who setup the sensors and enclosure and make the system ready for the customer to consume henceforth. He also shall be responsible contact for any field issues wrt to sensors and enclosures. We have intentionally not elaborated on this role in the document for the above assumption.
- We have assumed that local weather shall be available through publicly available Weather API. We have provided specific descriptions of such data records and related assumptions in later sections of this document.

## 2. Solution

In this section, we describe user personas and usage pattern analysis, which we used to help us make specific architectural choices. Notably, these analyses guided the particular components in our architecture. Subsequently, we specify the priority of order of architectural characteristics, followed by the proposed architectural style.

### 3.1 User Personas

It is essential to identify the needs of users who will directly use the FishWatch™ system. We have come up with user personas early in architecture development so that they can guide a product that will serve users' best interests.

#### John Doe, the farmer

John Doe is a Farmer who monitors the enclosure(s) in a farm. He is usually associated to 1 or more enclosure.
His job consists of :

- Regular rounds of the enclosures monitoring various situations  and report them further
- Dispensing feeds on regular intervals for the Farm animal.
- Assessing the farm animal growth and timing the harvest.
- His job also involves of medicines to the farm animal in case of any infection found.
- Responding to any exigency arising due to weather factors and other external factors.
He spends most of his day at the farm attending to needs of enclosure and other operations.
It would be very helpful for him, if the valuable information is available to him on regular basis for him to take appropriate action.

#### Charles Xavier, the Customer

Charles, is usually an owner of the farm, he is rarely at one location for long, if he has more than one farm. His time is spent monitoring and overseeing the operations of the farm. He usually employs the farmers to work in a farm
In a typical single farm or small farm, he is normally the person, who also works as a farmer. Hence information on the vital parameters at his fingertips, improves his productivity and also allows him to respond to the situation in a timely manner.

#### Natasha Romanoff, the Customer Administrator

In a farm, generally Customer Administrator is also a owner/Customer ,but in case of larger farms distributed across geography, a Customer administrator would be the farm in charge who is responsible for creating farm users in the system and allocating John Doe (farmer) an enclosure. Natasha also would be responsible for monitoring the farm health and review trends and identify potential issues in the farm.

#### Nick Fury, the Integrator

Nick sets up the LiveStock® FishWatch™ system , including setup of sensors, configuring them, training the farmers and customers to use the system. He could be a franchisee of the Livestock Insights / Employee of LiveStock Insights.

#### Tony Stark, the System Administrator

Tony is responsible for the understanding and upkeep of all of the hospital systems. He is available as a representative of the LiveStock Insights Inc .

- available at a notice to troubleshoot
- Ensure that system is available all the time
His errands include trying to stay ahead of issues and perform maintenance at times that will not stress out the function of systems. He could be notified in a disaster, but would prefer to have that situation covered ahead of time and not have to scramble to bring a system back up.

## 3.2 Usage Patterns

When we started designing our solution, we found it important to thoroughly understand how this system would be used.

- The Farmer can see the status of the each sensor and hub Status with the Fishwatch(R) app.
- Each sensor is considered independent in operation and does not affect the others in the event of a failure.
- The alerts must be dismissible, to keep the screen clear, hence there needs to be a functionality for acknowledge and the farmer can notify this to his admin on the account of device failure / take appropriate action in case of any other event.
- Farmer/admin  should also be able to view the past notifications in a separate window
- Administrator should be able to allocate farmer to a farm/ enclosure
- Screen for viewing the Trend over time

### Need for Admin Screens

With the above use cases, we were able to start organizing our architecture. We have several requirements that need user input, especially with Configuration of the rules and other administrative activities like Role based access and resource allocation to farmers.
By splitting the screen, we can treat the monitoring screen as a read-only display of data and set of admin screens available only to Customer Administrator and the Customer.
The new admin screen fields user inputs for configuring the sensors.
We can define the "Model DB / Digital Twin" component as an input for setup and analysis.

Admin Screens include

- _Configure Rules:_  Elaborated in the section below
- _Manage Users:_ This section has been deliberately omitted, as this contains the basic User configuration associating them with appropriate identified roles in the system
- _Firmware updates:_ this section also been deliberately omitted , as its planned for a later iteration
- _Manage Enclosures:_ This is meant to add/ remove sensors in the system, Device Provisioning and registration etc. There is no specific UI defined in this, it is assumed there exists a screen.

#### *FishWatch Monitor Screen*

We started by imagining the consolidated monitoring screen that is available at Farmers Device ( Rugged/ Mobile/PC ).
An **Enclosure** and a **Livestock** are two distinct data types. The **Enclosure**  represents a physical apparatus , that the sensors are fixed to. It also houses a **Hub**, which collects data from the sensors.

The Landing  screen shall give the farmer the most important snapshot of the current activity in the enclosure . He shall also be able to view Enclosure to enclosure through a carousel view.

![Dashboard](./Farmer_sm.png)

#### *Notification History/Alerts History*

Since we keep the Monitor screen neat and clean with only limited read only data, there also arises the need for showing the notification history and ability for the user to filter based on various filter conditions like

- Date range
- Enclosure
- Sensor Types

We imagined some useful notifications, and found that they fell into two categories:

- administrative alerts, which indicate simple status updates: devices disconnecting, Sensor Telemetry
- threshold alerts, which are complex to compute, which are more associative in nature or based on the rules that are configured at the Farm (Site)

This helped us figure out a key requirement for the telemetry database. It is critical that nothing impedes the sensors from writing to the telemetry database.
We decided to create a separate alerts database. Since the nature is more of writes, we chose a **Timeseries** database as the telemetry database
Splitting the database allows us to silo the sensor input database , where the rules are configured ( the digital Twin and rule DB ) to its core purpose: serving many concurrent writes, without a break.
All other functionality reads from here, processes the data, and sends it along. This split refined the design of the overall architecture. We created a clear delineation between the sensor database, and the analysis that comes after.

![NotificationHistory](./Notification_History_sm.png)

#### *Sensor Trends*

The customer administrator/farmer also needs to be able to configure dashboard with important sensor parameters for him to be able to view the trends. This helped us to arrive at conclusion, that there is a need to store telemetry as well as Alerts data for an increased period of time, hence this information is to be retained in the Cloud for future analysis for the customer as well as to run some ML Prediction models

![CustomDashboard](./Custom_Dashboard-sm.png)

#### *Configure Rules*

Apart from the regular telemetry, we thought it would be really powerful , if the Customer is notified on specific conditions, which involves coming up with a flexible way to setup thresholds.
The Rule Alert processor is integral to the alert processor, which on satisfying a certain condition shall initiate a new workflow.
Since this is always running and processing the incoming event/telemetry data, it drives us to define the system as an event-driven architecture for this portion of the system.

![Settings](./Settings-sm.png)

![ConfigureRules](./Rule_Threshold-sm.png)

#### *Manage Enclosures*

Manage enclosure is the core setup feature of the system, which includes activities like

- _Adding Enclosure_:
    Registering the Hub to the Fishwatch system
    Device provisioning  
- _Configure Sensors_:
    Sensor parameters and telemetry configuration

The Above Requirements drives us to the need of a device Provisioning service and a IOT Hub <<explain>>

#### Gateway Hub

The requirement of a Gateway Hub, came with need that cellular signals and connection to mainland is wavery.
Hence we need a Hub at the enclosure to collect the data locally and syncronizing them to Cloud , when the connectivity is restored.

### *3.3 Architecture Characteristics*

#### *Availability*

_Reason_:  Every region (EU, Americas,APAC etc) has its independent deployment of Telemetry DB , which are then aggregated globally for a multi national corporate. The System supports a small farm to real cross regional enterprise. Fishwatch primary purpose is to monitor enclosure health in real time, any system downtime could delay the detection of critical issues, leading to potential harm or even loss of business for its customers. High availability is also needed to ensure that alerts for abnormal conditions are delivered promptly, enabling customers to respond quickly in emergencies.

_Impact on Architecture:_

- We have added provision for load balancing in the data ingestion module.
- We have adopted Extract-Load-Transform (ELT) architecture for running ML Pipeline and analytics.

#### *Data Integrity*

_Reason_: Decision Support systems rely on accurate and reliable patient data. Therefore, FishWatch needs high data integrity, meaning the data across the system must be free from incorrect modification and loss.

_Impact on Architecture:_
    -   We have adopted shared databases, orchestrated through a telemetry pipeline ensuring reliable services.

#### *Data Consistency*

_Reason:_ Fishwatch system must also ensure that the sensor readings ingested, stored, and displayed reflect the current state of the Enclosure.As the nature of data is event driven, and data though important not life critical, we have chosen eventual consistency for telemetry data over ACID Compliant store.

_Impact on Architecture:_
    - We have adopted timeseries Document NOSQL DB as a shared database to be implemented on the cloud.

#### *Fault Tolerance*

_Reason:_ The Fishwatch system should maintain service while facing failures. The primary failure scenario is, when the enclosure lose connectivity to the Cloud / Sensors stop functioning/ or software components fail. It is essential for the Fishwatch system to still function for monitoring, recording, analyzing, and alerting based on the available data.
_Impact on Architecture:_

- We store and process each Telemetry timeseries independently of the other.
- We have designed for the ability to detect vital sign failures and the ability to alert a data/system administrator about the failures.
- We have also incorporated an ability to seamlessly ingest data after a failed component recovers, which essentially uses the distributed message broker architecture.

To tolerate failures, the system must consider redundancy and replication at various levels of the system. They however are assumed implicit and have little impact on the SW architecture.

#### Concurrency

_Reason:_ Fishwatch needs to read, store, and process data from multiple monitoring sources across multiple enclosures deployed across the globe . Therefore, it is necessary to design components to handle concurrent operations and improve performance.

_Impact on Architecture:_
    We have adopted a microservice architecture for data ingestion and analysis components.

#### Performance

_Reason:_ The system should be able to process requests on demand and be very responsive.The System also should be able to provide a consolidated view in the Monitor screen with avg response time <  2secs

_Impact on Architecture:_

- We have chosen a regional digital twin DB to maintain specific configuration.
- We have adopted interactions to be asynchronous where a response is not needed.
- We have designed the serverless services to handle concurrent requests and scale horizontally.

### 3.4 Architecture Style

We recommend a combination of microservice and event-driven architecture styles.

- Microservice architecture will allow keeping services of the system discrete, enabling fault tolerance and high availability.
- Event-driven architecture will enable real-time capabilities. Various components can subscribe to events and receive them as asynchronous messages. eg: a Live Alert can be immediately served to the User interface and parallelly this message can be queued and processed to the database, there by making it near real-time and decoupling them.
- As in microservices, we have minimised data sharing among microservices, The Event driven module ( Alerts and Notifications) uses Telemetry Databases, while Manage Enclosures and Rules uses a GraphDB as it allows us to define complex relationships . The shared database style is suitable because Fishwatch MonitorMe needs to prioritize data integrity and maintainability over data isolation .

## **4 System Architecture : Components**

### **4.1 System Architecture : Components**

#### Context

![Context](./C4%208-Context.drawio-sm.png)

#### Component

![Component](./C4-Component_diagram.jpg)

Based on the user persona and usage patterns analysis, we have broken system architecture into six high-level components:

1. Data Acquisition:

    - Interfaces with the various monitoring devices to retrieve real-time data on enclosures.
    - Buffering the data at Hub layer, till connectivity is restored
    - Ensures reliable and continuous data acquisition from all sources, handling potential errors or disruptions gracefully.
2. Device Interface :
    - Provision device on cloud during commissioning.
    - maintain device connection status
3. Data Ingestion/Processing :
    - Receives the raw sensor telemetry collected by the data acquisition component, stores, and processes it for further analysis.
    - Includes tasks such as data normalization, validation, aggregation, and analysis to derive meaningful insights from the raw data.
    - Performs real-time analysis to detect abnormalities or threshold violations in vital sign readings, triggering alerts for medical professionals.
4. Data Access/Config :
    - Responsible for presenting the processed sensor data to various screens in a user-friendly format.
    - Includes functionalities such as displaying sensor data on monitoring screens at Fishwatch Monitor and sending alerts to subscribers such as farmers/Administrator etc.
5. Data Administration:
    - Enter metadata such as thresholds and rules for generating alerts on vital sign signals.
    - Configure Sensor behaviors
    - User Management
    - Generate on-demand data snapshots and reports.
    - Generate historical reports.
6. Cross Cutting Concerns
    - Process ML Pipelines through Data Processing
    - Ingest External data to data lake like Weather Info.
    - Manage Authentication/Authorization
    - Maintain /Manage Master config database

### **4.2 System Architecture : Dataflow**

![Container Data Flow](./Container%201-DataFlow.drawio.png)

The system adheres to a specific data flow pattern to ensure efficient data processing and management. The data flow can be outlined as follows:

1. *Data Ingestion:*
    - Sensors installed in farm enclosures generate point values, each having an ID and a sensed value. Every gateway and sensor onboarded into the system is assigned a globally unique identifier. The point ID is a combination of the gatewayId and sensorId. These unique identifiers offer the following benefits:
    - All points sent to the cloud can be easily tracked using this ID.
    - When faulty sensors are replaced, these soft identifiers can be retained, ensuring historical continuity.
    - Configured cameras detect and report alerts.
    - Weather data from all farm locations is periodically extracted from external weather providers and stored in the data lake.
    - User configurations are persisted in the global digital twin store.

2. *Data Processing:*
    - The Gateway component performs real-time basic stream analytics on the collected telemetry data from all sensors, refers to the set configuration, and generates alerts.
    - Advanced analytics services process the data, detect anomalies, and generate alerts.
    - Reported inclement weather conditions also trigger alert generation.

3. *Data Storage:*
    - Telemetry data is stored in regionally deployed time series databases, considered as the "System of Records".
    - The globally deployed data lake house periodically pulls data from the regional time series databases. This global data lake house is the derived database.
    - The global graph database stores the model (digital twin) information, device configurations, and user preferences.
    - Regionally deployed model databases create a copy of the model information by processing the change feed/change data capture events from the global graph database.
    - Upon successful saving of rule configurations on sensors and cameras in the model database, the same configurations are downloaded to gateways for storage.

4. *Data Output:*
    - The analyzed data or derived results are presented to end-users or other systems in a consumable format, such as reports, dashboards, APIs, or other data delivery methods.
    - Generated alerts are sent to users as email, SMS, or push notifications, according to the user's configured preferences for alert notifications.

## 5. Detailed Architecture

### **5.1 Fish watch Data Model**

The main data model of Fishwatch is split into 2 types and we have 3 varieties of Data Stores.

1. **Fishwatch DataModel**
    The Fishwatch data model is envisioned as a Graph Database where the relationships are maintained and all the data administrative data is maintained.
    The relationships are defined as depicted in the diagram below:

    ![GraphModel](./C4%2010-GraphModel.png)

*Rationale*: The Decision for the GraphDB is recorded in the [ADR_GraphDB](./adr_graphdb.md)

*Note*: The System is designed extensible, the Same model can be extended to other livestocks with a similar setup,

2. **Telemetry DataModel**

We chose to follow Cloudevents model for storing Telemetry events in the system. the Data Processor module converts and stores the data in the below Model. The Schema is loosely defined as below.
The data is stored in the telemetry database which is a timeseries documennt DB hosted as a PaaS service in the Cloud.

|CloudEvents| Type| Exemplary JSON Value|
|-----------|-------|-------------------------|
|type |String| "com.example.someevent"|
|specversion| String| "1.0"
|source| |URI-reference| "/mycontext"|
|subject| |String| "larger-context"|
|subject| |String (null)| null|
|id |String |"1234-1234-1234"|
|time| |Timestamp| "2018-04-05T17:31:00Z"|
|time| |Timestamp| (null) null|
|datacontenttype |String| "application/json"|

The Cloud Events model is explained in detail in the document link [CloudEvents Model](./Cloudevents-json-format.md)

*Rationale*:

- The decision for selecting CloudEvents model is recorded in the [ADR_CloudEvents](./adr_CloudEvents.md).
- Timeseries Database selection is recorded in the [Timeseries ADR](./adr_timeseriesdatabase_for_storing_telemetry.md)

### **5.2 Microservice Independence**

Each Important microservice shall maintain a model copy of relevant information to maintain their independence. This is achieved through: 

- Model Service emits Change Data Capture (CDC) events to a message broker, which Notification service and Alert processing services subscribes to. 
- Once CDC events are received by these 2 services, they transform them as needed before persisting them. This design ensures loose coupling and independent scalability of both services.

This ensures, that there is a single Source of truth as Model DB, but relevant services shall have its own local copy as per their needs.

The detailed flow for creating local copies that can be independently scaled along with the owner service is shown below: 

![DistributedModel](./Microservice_independence.jpg)


### 5.3 Microservice Descriptions

#### **Overview**

In our system architecture, we have implemented several microservices to handle specific functionalities. Each microservice is designed to serve a specific purpose and contribute to the overall system's functionality.

#### **API Gateway**

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

#### **Device access and Config layer (deployed globally)**

##### **Gateway Provisioning Service**

*Premise:*

Each manufactured gateway comes pre configured with authentication credentials and is assigned a specific Gateway Provisioning Service and API endpoint. The Gateway Provisioning Service also updates its enrollment list accordingly.

*Purpose**:

- When the gateway is powered on for the first time, it connects to the Gateway Provisioning Service endpoint and presents its authentication credentials.
- The Gateway Provisioning Service verifies the gateway's identity against its enrollment list.
- Once the gateway's identity is confirmed, the service assigns the gateway device to an IoT hub and registers it in the hub based on the region of the farm to which the gateway is associated.
- The Gateway Provisioning Service receives the unique gateway device ID and registration information from the assigned hub and relays this information back to the gateway device. The gateway then uses its registration information to connect directly to its assigned IoT hub and authenticate itself.
- Following successful authentication, the gateway and IoT hub commence direct communication. The Gateway Provisioning Service's role as an intermediary concludes at this point, unless the gateway requires re provisioning in the future.

##### Onboarding API

*Purpose*:
This API provides all the essential endpoints for adding a farm company, farm, enclosure, gateway, sensors, cameras, and users.
![Onboarding](./C4-Customer_Onboarding.jpg)

##### Configuration API

*Purpose*:

- This API houses all the necessary endpoints for associating a gateway with a farm, enabling a user to save their notification preferences, setting sensor thresholds for alerts, choosing preferred metrics or details to be displayed in the web interface, selecting a preferred language, and determining the preferred units.

##### Insights API

*Purpose*:

- This API is built on GraphQL (for reasons behind choosing GraphQL, refer to the ADR: [InsightsAPI](./adr_insightsAPI_graphql.md)). Upon successful user login, the API retrieves all necessary details for the farms the user has access to and organizes these farms based on their respective regions. It then initiates synchronous API calls to all regional APIs to gather the required data, such as alarms or telemetry. This data is consolidated and relayed to the caller. However, it's crucial to note that tail-end latencies can potentially impact the performance of this API. Additionally, this API is equipped to fetch insights generated by the advanced analytics service.

##### Model Service

*Purpose*:

This acts as a facade for the digital twin data stored in the graph database (for reasons behind choosing a Graph database, refer to the  ADR :[GraphDB_ADR](./adr_graphdb.md)). It offers options to create various vertices, edges, and their corresponding attributes, facilitating efficient querying and retrieval of required results.
Additionally, this service publishes change feed or change data capture events to a message broker. This allows other services that rely on digital twin information to build their own copies in the format that best suits their needs. This feature enhances the scalability, flexibility and adaptability of the system, ensuring that all services can effectively utilize the digital twin data.

##### Notification Service

*Purpose*:

- This service operates as a background service that continuously listens to a message broker, such as an Azure Event Hub.
- Alert processing services, deployed in various regions, publish alerts to this same message broker. Similarly, weather data ingestion services in different regions publish alerts related to inclement weather conditions to the same broker.
- The Global Advanced Analytics service also publishes any alerts based on detected anomalies to this message broker.

The Notification Service processes these alert messages, refers to the model service to enrich the context, and sends notification alerts based on user-configured preferences. This ensures that all relevant alerts are efficiently processed and delivered according to user preferences, providing a comprehensive and personalized alert system.

##### Advanced Analytics Service

*Purpose*:

- This service includes a data lake house that aggregates all telemetry data from regional deployments, as well as a machine learning (ML) training component. It also maintains a persistent database to store insights for future use.
- The service features an API that utilizes the trained model to predict the yield based on a given set of parameters.
- Furthermore, if the service identifies any anomalies, it promptly sends alerts to the notification service. This ensures immediate awareness and facilitates timely response to potential issues.

#### Data Ingestion Layer

##### Telemetry Ingestion Service

*Purpose*: This service processes the stream of telemetry data (coming from IoT hub) related to points and their values, and persists them in the time series database.

##### Alert Processing Service

*Purpose*: This service monitors alert messages sent by the gateway as part of the telemetry. It enriches these alerts by referring to its own replica of the digital twin model database, stores them in the time series database, and then publishes them to a message broker (like Azure Event Hub). This allows the alerts to be processed by the global notification service.

##### Weather Data Ingestion Service

*Purpose*: This is a continuously running background job that fetches weather information for all locations where farms are situated. The list of farms and their locations is retrieved from its own replica of the digital twin model database. The service then stores this weather data into the time series database.

### Conclusion

These microservices work together to provide a scalable and modular architecture, allowing us to efficiently handle different functionalities within our distributed system.

