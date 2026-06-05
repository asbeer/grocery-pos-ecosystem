# Project Structure Overview

## Root Directory Layout

```
grocery-pos-ecosystem/
├── backend-api/                    # ASP.NET Core API
│   ├── GroceryPOS.API/
│   ├── GroceryPOS.Core/
│   ├── GroceryPOS.Infrastructure/
│   ├── GroceryPOS.Services/
│   ├── Tests/
│   └── appsettings.json
│
├── windows-pos-app/               # Windows Desktop Application
│   ├── GroceryPOS.Windows/
│   ├── GroceryPOS.Windows.Core/
│   ├── GroceryPOS.Windows.UI/
│   ├── GroceryPOS.Windows.Services/
│   └── GroceryPOS.Windows.sln
│
├── android-app/                   # Android Mobile App
│   ├── app/
│   │   ├── src/main/
│   │   │   ├── java/com/grocerypos/
│   │   │   └── res/
│   │   └── build.gradle.kts
│   ├── gradle/
│   └── build.gradle.kts
│
├── database-schema/               # Database Design
│   ├── migrations/
│   ├── procedures/
│   ├── seed-data/
│   └── schema.sql
│
├── docs/                          # Documentation
│   ├── API_DOCUMENTATION.md
│   ├── DATABASE_SCHEMA.md
│   ├── DEPLOYMENT.md
│   ├── ARCHITECTURE.md
│   ├── HARDWARE_SETUP.md
│   └── USER_GUIDE.md
│
├── docker-compose.yml
├── .github/
│   └── workflows/
│       ├── build-windows.yml
│       ├── build-android.yml
│       └── deploy-backend.yml
├── README.md
└── .gitignore
```

## Branch Strategy

- **main** - Production release branch
- **backend-api** - Backend development
- **windows-pos-app** - Windows app development
- **android-app** - Android app development
- **database-schema** - Database migrations & schema
- **deployment** - Docker & cloud configs

## Development Workflow

1. Create feature branches from respective component branches
2. All PRs must include tests and documentation
3. CI/CD pipeline validates builds before merge
4. Main branch auto-deploys to production
