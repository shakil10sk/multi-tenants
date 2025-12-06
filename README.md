
# ER Diagram (Entities & Relationships)

## erDiagram
    ```
        USERS ||--o{ HOUSE_OWNERS : manages
        HOUSE_OWNERS ||--o{ FLATS : owns
        HOUSE_OWNERS ||--o{ BILL_CATEGORIES : defines
        HOUSE_OWNERS ||--o{ BILLS : issues
        FLATS ||--o{ BILLS : receives
        FLATS ||--o{ TENANTS : houses
        BILLS ||--o{ PAYMENTS : receives
        USERS ||--o{ PAYMENTS : makes
    ```

## Explanation of relationships:
    * Admin (user.role=admin) manages House Owners and optionally Tenants.
    * House Owner owns Flats, Bill Categories, Bills, and manages Tenants assigned to their flats.
    * Flats have Tenants and can receive Bills.
    * Bills may have Payments (by tenants or house owner).
    * Users table stores Admins and House Owners (login capable). Tenants may or may not have login.


# Architecture Diagram (System Overview)

## flowchart LR
    ```
        Client[Browser / SPA / React] -->|HTTPs| LB[Load Balancer]
        LB --> App[Laravel API (stateless, JWT auth) x N]
        App --> DB[(Postgres Primary)]
        App --> ReadReplica[(Read Replica(s))]
        App --> Redis[(Redis - cache & queue)]
        App --> Mail[(SMTP / SES)]
        App --> ObjectStore[(S3 / Storage for attachments)]
        QueueWorker[Queue Workers] --> Redis
        QueueWorker --> Mail
        AdminConsole[Admin Web UI] --> LB
    ````

## Notes:
    - Client SPA calls Laravel API using JWT in Authorization: Bearer header.
    - Laravel API applies TenantScope to all queries for House Owner users. Admins bypass TenantScope.
    - Redis used for queues and caching frequently-accessed data.
    - Queue Workers process emails asynchronously.
    - Read Replica used for heavy read queries like bills listing.