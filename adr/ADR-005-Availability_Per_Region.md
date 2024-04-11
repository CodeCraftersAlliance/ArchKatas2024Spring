# Availability Setup per region

## Status

Accepted

## Context

There are farms located in various Geographical region across the globe - We have to make a choice whether to have a one single centrally available Fishwatch System or regionally available system.


## Decision (decision and justification - the "why")

We decided to process the sensor and gateway data as much as close to the origin, reason being to reduce the latency incurred during transit and data processing centrally. Also the critical sensor exceeding the threshold should be reported immediately to the farmer nearest to the region, processing it centrally we cannot guarantee the priority for a given region. Any down time for the regional services will have the potential impact on the health of the farm hence we have provision for load balancing built for the services in Data Ingestion for high Availability.

## Consequences (tradeoff information and any other notable side effects, also impacts)
This will have the positive impact on the availability of the fishwatch system with a tradeoff on cost in building regional end points.
