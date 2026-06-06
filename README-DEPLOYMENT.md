# Deployment Guide - Docker & Cloud

Complete deployment guide for the Grocery Store POS Ecosystem.

## Quick Start with Docker Compose

```bash
git checkout deployment
docker-compose up -d
```

## Services

### Backend API
- Container: grocerypos-api
- Port: 5001
- Restart: always

### PostgreSQL Database
- Container: grocerypos-db
- Port: 5432
- Volume: grocery_pos_data

### Redis Cache
- Container: grocerypos-redis
- Port: 6379
- Restart: always

### pgAdmin
- Container: grocerypos-pgadmin
- Port: 5050
- Access: http://localhost:5050

## Configuration

### Environment Variables

```bash
# API
ASPNETCORE_ENVIRONMENT=Production
ASPNETCORE_URLS=https://+:5001
ConnectionStrings__DefaultConnection=Server=db;Port=5432;Database=grocery_pos;User Id=postgres;Password=postgres;
JwtSettings__Secret=your-256-bit-secret-key-here
JwtSettings__ExpirationMinutes=60

# Database
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=grocery_pos

# Redis
REDIS_PASSWORD=redis-password
```

## Docker Compose Configuration

```yaml
version: '3.8'

services:
  api:
    image: grocerypos-api:latest
    ports:
      - "5001:5001"
    environment:
      - ConnectionStrings__DefaultConnection=Server=db;Port=5432;Database=grocery_pos;User Id=postgres;Password=postgres;
    depends_on:
      - db
      - redis
    networks:
      - grocerypos-network

  db:
    image: postgres:15-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: grocery_pos
    volumes:
      - grocery_pos_data:/var/lib/postgresql/data
    networks:
      - grocerypos-network

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    networks:
      - grocerypos-network

  pgadmin:
    image: dpage/pgadmin4
    ports:
      - "5050:80"
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@example.com
      PGADMIN_DEFAULT_PASSWORD: admin
    networks:
      - grocerypos-network

networks:
  grocerypos-network:
    driver: bridge

volumes:
  grocery_pos_data:
```

## Deployment Steps

### 1. Prerequisites
- Docker & Docker Compose installed
- SSH access to server
- Domain name configured
- SSL certificate ready

### 2. Clone Repository
```bash
git clone https://github.com/asbeer/grocery-pos-ecosystem.git
cd grocery-pos-ecosystem
git checkout deployment
```

### 3. Configure Environment
```bash
# Copy example env file
cp .env.example .env

# Edit with your values
nano .env
```

### 4. Build & Start Services
```bash
# Build API image
docker build -t grocerypos-api:latest -f Dockerfile .

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f api
```

### 5. Database Migration
```bash
# Run migrations
docker exec grocerypos-api dotnet ef database update

# Seed initial data
docker exec grocerypos-api dotnet run --seed
```

### 6. Verify Deployment
```bash
# Check API health
curl https://localhost:5001/health

# Check database
docker exec grocerypos-db psql -U postgres -d grocery_pos -c "SELECT 1"

# Check Redis
docker exec grocerypos-redis redis-cli ping
```

## Kubernetes Deployment

### Prerequisites
- kubectl installed
- Kubernetes cluster (EKS, AKS, GKE, etc.)
- Helm (optional)

### Deploy to Kubernetes
```bash
# Create namespace
kubectl create namespace grocerypos

# Apply configurations
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secrets.yaml
kubectl apply -f k8s/pvc.yaml
kubectl apply -f k8s/db-deployment.yaml
kubectl apply -f k8s/redis-deployment.yaml
kubectl apply -f k8s/api-deployment.yaml
kubectl apply -f k8s/ingress.yaml

# Check status
kubectl get pods -n grocerypos
```

## Cloud Deployment

### AWS (ECS/Fargate)
```bash
# Create ECR repository
aws ecr create-repository --repository-name grocerypos-api

# Push image
docker tag grocerypos-api:latest <account-id>.dkr.ecr.<region>.amazonaws.com/grocerypos-api:latest
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/grocerypos-api:latest

# Deploy task
aws ecs create-task-definition --family grocerypos-api --container-definitions file://task-definition.json
aws ecs create-service --cluster grocerypos --service-name api --task-definition grocerypos-api
```

### Azure (Container Instances)
```bash
# Push to ACR
az acr build --registry <registry-name> --image grocerypos-api:latest .

# Deploy
az container create --resource-group <rg> --name grocerypos-api --image <registry>.azurecr.io/grocerypos-api:latest
```

### Google Cloud (Cloud Run)
```bash
# Build and push
gcloud builds submit --tag gcr.io/<project>/grocerypos-api

# Deploy
gcloud run deploy grocerypos-api --image gcr.io/<project>/grocerypos-api --platform managed
```

## Monitoring & Logging

### Logging
```bash
# View API logs
docker-compose logs -f api

# View database logs
docker-compose logs -f db

# View all logs
docker-compose logs
```

### Metrics
- CPU usage
- Memory usage
- Database connections
- API response time
- Error rates

## Backup & Recovery

### Database Backup
```bash
# Backup
docker exec grocerypos-db pg_dump -U postgres grocery_pos > backup.sql

# Restore
docker exec -i grocerypos-db psql -U postgres grocery_pos < backup.sql
```

### Volume Backup
```bash
# Backup volume
docker run --rm -v grocery_pos_data:/data -v $(pwd):/backup alpine tar czf /backup/data-backup.tar.gz -C /data .

# Restore volume
docker run --rm -v grocery_pos_data:/data -v $(pwd):/backup alpine tar xzf /backup/data-backup.tar.gz -C /data
```

## SSL/TLS Configuration

### Self-signed Certificate
```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

### Let's Encrypt with Nginx
```bash
certbot certonly --standalone -d yourdomain.com
```

## Troubleshooting

### Common Issues

**API Won't Start**
- Check environment variables
- Check database connection
- Check logs: `docker-compose logs api`

**Database Connection Failed**
- Verify database is running
- Check connection string
- Verify PostgreSQL credentials

**Port Already in Use**
- Change port in docker-compose.yml
- Kill process: `lsof -i :5001`

## Documentation

See main README.md for complete project documentation.
