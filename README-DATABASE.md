# Database Schema - PostgreSQL

Complete PostgreSQL database schema for Grocery Store POS Ecosystem.

## Tables

### Users
- id (PK)
- username (UNIQUE)
- password_hash
- email
- full_name
- phone_number
- role_id (FK)
- is_active
- created_at
- last_login
- refresh_token
- refresh_token_expiry

### Roles
- id (PK)
- name
- description
- created_at

### Products
- id (PK)
- name
- description
- barcode (UNIQUE)
- category_id (FK)
- price
- cost_price
- reorder_level
- supplier_id (FK)
- is_active
- created_at
- expiry_date

### Categories
- id (PK)
- name
- description
- is_active
- created_at

### Inventory
- id (PK)
- product_id (FK)
- quantity
- reserved_quantity
- last_stocked_at
- batch_number
- expiry_date

### Orders
- id (PK)
- order_number (UNIQUE)
- customer_id (FK)
- cashier_id (FK)
- subtotal
- tax_amount
- discount_amount
- total_amount
- status (Pending, Completed, Cancelled)
- order_type (WalkIn, CarOrder, Delivery)
- created_at
- completed_at
- notes

### OrderItems
- id (PK)
- order_id (FK)
- product_id (FK)
- quantity
- unit_price
- line_total
- notes

### Customers
- id (PK)
- name
- email
- phone_number
- loyalty_points
- credit_limit
- credit_used
- created_at
- is_active

### Payments
- id (PK)
- order_id (FK)
- payment_method (Cash, Card, Online)
- amount
- reference_number
- status (Pending, Completed, Failed)
- created_at

### Deliveries
- id (PK)
- order_id (FK)
- driver_id (FK)
- status (Pending, InTransit, Completed, Failed)
- scheduled_date
- pickup_time
- delivery_time
- pickup_location
- delivery_location
- latitude
- longitude
- created_at

### Drivers
- id (PK)
- name
- phone_number
- vehicle_number
- is_active
- created_at

### Suppliers
- id (PK)
- name
- contact_person
- phone_number
- email
- address
- payment_terms
- is_active
- created_at

## Setup

```bash
# Create database
createdb grocery_pos

# Apply schema
psql grocery_pos < schema.sql

# Apply migrations
dotnet ef database update
```

## Indexes

- orders(customer_id)
- orders(cashier_id)
- orders(created_at)
- products(barcode)
- products(category_id)
- inventory(product_id)
- orderitems(order_id)
- orderitems(product_id)
- deliveries(driver_id)
- deliveries(status)

## Documentation

See main README.md for complete project documentation.
