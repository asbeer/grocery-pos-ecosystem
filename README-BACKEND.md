# Grocery Store POS - Backend API

## Overview

ASP.NET Core 8.0 REST API with SignalR for real-time synchronization between Windows POS and Android mobile app.

## Architecture

```
GroceryPOS.API
├── Controllers (REST endpoints)
├── Hubs (SignalR real-time communication)
├── Program.cs (Startup configuration)
└── appsettings.json (Configuration)

GroceryPOS.Core
├── Entities (Database models)
└── DTOs (Data transfer objects)

GroceryPOS.Infrastructure
├── Data (DbContext)
└── Repositories (Data access layer)

GroceryPOS.Services
└── Business logic services
```

## Setup

### Prerequisites
- .NET 8.0 SDK
- PostgreSQL 14+
- Visual Studio 2022 or VS Code

### Installation

```bash
# Clone repository
git clone https://github.com/asbeer/grocery-pos-ecosystem.git
cd grocery-pos-ecosystem
git checkout backend-api

# Restore packages
dotnet restore

# Apply database migrations
dotnet ef database update

# Run API
dotnet run
```

## API Endpoints

### Authentication
- `POST /api/auth/login` - Login with username/password
- `POST /api/auth/refresh` - Refresh JWT token

### Products
- `GET /api/products` - Get all products (paginated)
- `GET /api/products/barcode/{barcode}` - Get product by barcode
- `POST /api/products` - Create product (Admin)
- `PUT /api/products/{id}` - Update product (Admin)

### Orders
- `GET /api/orders/{id}` - Get order details
- `POST /api/orders` - Create new order
- `PUT /api/orders/{id}` - Update order

### Customers
- `GET /api/customers/{id}` - Get customer
- `GET /api/customers/search` - Search customers
- `POST /api/customers` - Create customer

### Inventory
- `GET /api/inventory/{productId}` - Get inventory
- `GET /api/inventory/low-stock` - Get low stock items
- `PUT /api/inventory/{productId}` - Update quantity

### Deliveries
- `GET /api/deliveries/{id}` - Get delivery
- `GET /api/deliveries/pending` - Get pending deliveries
- `POST /api/deliveries` - Create delivery

## Real-Time Hubs (SignalR)

### Order Hub (`/hubs/orders`)
- `OrderCreated` - New order created
- `OrderUpdated` - Order updated
- `OrderStatusChanged` - Order status changed

### Inventory Hub (`/hubs/inventory`)
- `InventoryUpdated` - Stock quantity changed
- `LowStockAlert` - Low stock alert

### Delivery Hub (`/hubs/delivery`)
- `DeliveryLocationUpdated` - Driver location update
- `DeliveryStatusChanged` - Delivery status changed

## Configuration

### appsettings.json
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Port=5432;Database=grocery_pos;User Id=postgres;Password=postgres;"
  },
  "JwtSettings": {
    "Secret": "your-256-bit-secret-key",
    "ExpirationMinutes": 60,
    "RefreshTokenExpirationDays": 7
  }
}
```

## Database

### Models
- **Users** - System users with roles
- **Roles** - User roles and permissions
- **Products** - Product catalog
- **Categories** - Product categories
- **Inventory** - Stock management
- **Orders** - Sales orders
- **OrderItems** - Line items
- **Customers** - Customer data
- **Payments** - Payment records
- **Deliveries** - Delivery tracking
- **Drivers** - Driver management

## Security

- JWT token-based authentication
- Role-based authorization
- Password hashing with BCrypt
- HTTPS enforcement
- CORS configuration
- Input validation with FluentValidation

## Deployment

### Docker
```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY GroceryPOS.API/bin/Release/net8.0/publish .
EXPOSE 5000
ENTRYPOINT ["dotnet", "GroceryPOS.API.dll"]
```

### Environment Variables
```bash
ConnectionStrings__DefaultConnection=postgresql://user:password@localhost:5432/grocery_pos
JwtSettings__Secret=your-secret-key
JwtSettings__ExpirationMinutes=60
```

## Development

### Add Migration
```bash
dotnet ef migrations add MigrationName
dotnet ef database update
```

### Running Tests
```bash
dotnet test
```

## Documentation

Swagger documentation available at: `https://localhost:5001/swagger`
