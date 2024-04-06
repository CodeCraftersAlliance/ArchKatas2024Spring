## Data Flow

The system adheres to a specific data flow pattern to ensure efficient data processing and management. The data flow can be outlined as follows:
1.	Data Ingestion:
    - Sensors installed in farm enclosures generate point values, each having an ID and a sensed value. Every gateway and sensor onboarded into the system is assigned a globally unique identifier. The point ID is a combination of the gatewayId and sensorId. These unique identifiers offer the following benefits:
        - All points sent to the cloud can be easily tracked using this ID.
        - When faulty sensors are replaced, these soft identifiers can be retained, ensuring historical continuity.
    - Configured cameras detect and report alerts.
    - Weather data from all farm locations is periodically extracted from external weather providers and stored in the data lake.
    - User configurations are persisted in the global digital twin store.

2.	Data Processing:
    - The Gateway component performs real-time basic stream analytics on the collected telemetry data from all sensors, refers to the set configuration, and generates alerts.
    - Advanced analytics services process the data, detect anomalies, and generate alerts.
    - Reported inclement weather conditions also trigger alert generation.

3.	Data Storage:
    - Telemetry data is stored in regionally deployed time series databases, considered as the "System of Records".
    - The globally deployed data lake house periodically pulls data from the regional time series databases. This global data lake house is the derived database.
    - The global graph database stores the model (digital twin) information, device configurations, and user preferences.
    - Regionally deployed model databases create a copy of the model information by processing the change feed/change data capture events from the global graph database.
    - Upon successful saving of rule configurations on sensors and cameras in the model database, the same configurations are downloaded to gateways for storage.

4.	Data Output:
    - The analyzed data or derived results are presented to end-users or other systems in a consumable format, such as reports, dashboards, APIs, or other data delivery methods.
    - Generated alerts are sent to users as email, SMS, or push notifications, according to the user's configured preferences for alert notifications.