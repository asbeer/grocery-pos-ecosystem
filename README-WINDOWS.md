# Grocery Store POS - Windows Desktop Application

Professional .NET WPF desktop application for supermarket retail operations with real-time synchronization, offline capability, and comprehensive hardware integration.

## Key Features

✅ Real-time POS Billing with barcode scanning  
✅ Hardware integration (printers, scanners, cash drawer)  
✅ Offline-first with automatic synchronization  
✅ Multi-user with role-based access control  
✅ Inventory management and tracking  
✅ Customer management and loyalty programs  
✅ Comprehensive reporting and analytics  
✅ Arabic & English multi-language support  

## Technology Stack

- .NET 8.0 WPF
- MVVM Architecture
- SignalR Real-time Sync
- SQLite Local Database
- Prism Framework

## Setup

```bash
git checkout windows-pos-app
dotnet restore
dotnet run --project GroceryPOS.Windows
```

## Build Installer

```bash
dotnet publish -c Release -r win-x64 --self-contained
```

## Key Components

### Services
- **ApiService** - HTTP communication with backend
- **SyncService** - SignalR real-time synchronization
- **PrinterService** - Hardware printer integration
- **DatabaseService** - Local SQLite operations

### Views
- LoginWindow - User authentication
- POSWindow - Point of sale interface
- InventoryWindow - Stock management
- CustomerWindow - Customer information
- ReportsWindow - Sales analytics

## Security

- JWT token authentication
- Role-based access control
- Encrypted local storage
- Audit logging
- HTTPS communication

## Documentation

See main README.md for complete project documentation.
