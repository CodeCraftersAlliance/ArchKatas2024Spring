# Local Storage In Gateway

## Status

Accepted

## Context

Gateway need a mechanism to persist the data to address the following use cases
1. How to maintain sensor level configuration 
2. How to make an informed decision to raise an alam if the threshold value exceeds ?
3. During connectivity lost to the head end system, how to store the telemetry data of a sensor ?


## Decision (decision and justification - the "why")
Introducing local storage to address all above stated use cases, this local gateway level storage going to persist like head end system info, sensor info, sensor specific thershold setting info and telemetry data with retention of 5 day or 200 MB data whichever earlier.

### Asumption to retention of telemetric data
1. Assumed each gateway can associate with 20 sensor and every sensor polling telemetric data for every 1 min with 1 kb size of data.
2. 24 * 60 * 1kb * 20 = 28800Kb(28MB) 


## Consequences (tradeoff information and any other notable side effects, also impacts)
Pros: In the event of connectivity lost with headend system due to poor network, locally stored information can utilized by simplified user interface to know abouth the basic status of closures.
