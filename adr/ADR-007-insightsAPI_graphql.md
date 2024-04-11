# Architecture Decision Log
## **Decision: Adoption of GraphQL for Insights API**
**Context**:
- The system requires an API that allows users of the web application to customize the metrics or data they want to see upon login. The data/metrics used for rendering mobile and desktop views differ significantly. Mobile views require only 2 or 3 metrics, while desktop views can load more widgets and thus fetch more data from the API.

**Considered Alternatives:**
- RESTful API was considered as an alternative. However, it was not chosen due to its rigid structure and the potential for over-fetching or under-fetching data.

**Decision:**
- We decided to use a GraphQL interface for our Insights API. GraphQL allows clients to specify exactly what data they need, which makes it a good fit for our requirements.

**Rationale**:
- By using GraphQL, we can efficiently load data for both mobile and desktop views. This approach reduces over-fetching and under-fetching issues, as each client specifies what it needs. This decision also allows us to provide a more customizable user experience, as users can select the metrics they want to see in the dashboards. 

**Status**:
Decided