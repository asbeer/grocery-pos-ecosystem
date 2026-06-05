# System Architecture

## Overview

The Grocery Store POS Ecosystem is a three-tier enterprise architecture with real-time synchronization capabilities.

## Architecture Layers

### 1. Presentation Layer

**Windows Desktop Application**
- .NET WPF with MVVM architecture
- Touchscreen-optimized UI
- Offline-first with local SQLite cache
- Real-time sync via SignalR

**Android Mobile Application**
- Kotlin with Jetpack Compose
- Room Database for offline persistence
- WorkManager for background sync
- Real-time updates via Firebase Cloud Messaging

### 2. Application Layer (Backend API)

**ASP.NET Core REST API**
- RESTful endpoints for all operations
- JWT authentication & authorization
- Role-based access control (RBAC)
- Input validation & error handling
- Rate limiting & throttling

**Real-Time Communication**
- SignalR hub for Windows client
- WebSocket for direct communication
- Firebase Cloud Messaging for Android push notifications

**Services**
- Business logic services
- Order processing
- Inventory management
- Payment processing
- Reporting & analytics
- Notification service

### 3. Data Layer

**PostgreSQL Database**
- Relational data model
- ACID transactions
- Automatic backups
- Replication support

**Local Caches**
- Windows: SQLite offline cache
- Android: Room Database
- Redis (optional): Session & distributed cache

## Data Flow

```
Windows POS          Android App
    ↓                   ↓
    └─→ SignalR/REST ←─┘
            ↓
    ASP.NET Core API
            ↓
        PostgreSQL
```

## Synchronization Strategy

### Real-Time Sync
1. Client makes change (add item, update order)
2. Sends to API via HTTP POST
3. API updates database
4. SignalR broadcasts to all connected clients
5. Clients update UI automatically

### Offline Sync
1. Changes made locally (SQLite/Room)
2. Queued for sync when offline
3. When connection restored, background job syncs changes
4. Conflict resolution via last-write-wins or timestamp-based
5. Reconciliation with server state

## Security Architecture

- **Authentication**: JWT tokens with refresh rotation
- **Authorization**: Role-based permissions
- **Encryption**: TLS/SSL for all communication
- **Data Protection**: Encrypted PII at rest
- **Audit**: Complete action logging
- **Secrets Management**: Environment variables & secure vaults

## Scalability Considerations

- **Horizontal**: Multiple API instances behind load balancer
- **Vertical**: Database connection pooling
- **Caching**: Redis for high-frequency queries
- **CDN**: Static asset delivery
- **Message Queue**: RabbitMQ/Azure Service Bus for async operations

## Deployment Architecture

**Cloud Deployment** (Docker containers)
- API microservice
- PostgreSQL database
- Redis cache
- Background job processor
- Message broker

**Local Deployment**
- Windows installer with embedded SQLite
- Android APK with Room Database
- Optional local API server
