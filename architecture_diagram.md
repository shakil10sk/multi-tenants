## flowchart LR
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


##Notes:

Client SPA calls Laravel API using JWT in Authorization: Bearer header.

Laravel API applies TenantScope to all queries for House Owner users. Admins bypass TenantScope.

Redis used for queues and caching frequently-accessed data.

Queue Workers process emails asynchronously.

Read Replica used for heavy read queries like bills listing.