## Table of Contents

1. [Functional Requirements](#functional-requirements)
    - [Data Ingestion](#data-ingestion)
    - [Data Consolidation and Display](#data-consolidation-and-display)
    - [Data Recording and Storage](#data-recording-and-storage)
    - [Data Analysis and Alerting](#data-analysis-and-alerting)
    - [User Interface and Interaction](#user-interface-and-interaction)
    
2. [Non Functional Requirements](#non-functional-requirements)
3. [Section 3](#section-3)

## Functional Requirements

### Data Ingestion
The data ingestion process involves reading data from various sensors and sources, such as water temperature, pH, dissolved oxygen, ammonia, turbidity, camera feeds, and weather information. This data should be collected at regular intervals, such as every 5 minutes for sensor data and hourly for weather information. The system should be able to handle this data and store it for further processing and analysis.

### Data Consolidation and Display
The system should be designed to consolidate data from these multiple sources and display it to users who log into the Fishwatch web application. This will allow them to monitor the health of the farms remotely, provided the farm is online.
In the event that a farm loses its cloud connectivity, users should be able to connect to the gateway via the access point hosted on the installed gateway at the farm, ensuring secure access.

### Data Recording and Storage
Fishwatch is required to record and store all telemetry data (including sensor readings and alarms), as well as weather information for a minimum of three years. This stored information is crucial for displaying real-time data to users via the web application and for training machine learning models to predict yield and provide valuable insights to farm owners.

### Data Analysis and Alerting
The system should be capable of analyzing each sensor value for abnormalities or preset thresholds. When such cases are detected by the gateways installed on the farm, alerts should be sent to the cloud. Depending on the notification preferences set by the user, these alerts will be dispatched as SMS or email notifications to the users.

### User Interface and Interaction
Fishwatch should offer an intuitive user interface for farm owners and enterprise customers (those owning multiple farms) to view the dashnboards as per the user's preference. It should enable farm owners to configure alert thresholds, view real-time sensor data, access historical records, weather information, and other useful insights.

## Non Functional Requirements

Content for section 3 goes here.