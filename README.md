# Retail Shop Billing System

A complete Spring Boot retail billing system with product inventory management, bill generation, discount/GST calculation, and daily sales reports.

## Features

- **Product Management**: Add, edit, delete products with stock management
- **Billing System**: Create bills with multiple items, automatic calculations
- **Discount & GST**: Apply percentage-based discounts and GST (18% default)
- **Daily Sales Report**: View today's total sales and bill count
- **Bill History**: View all bills with customer details and amounts
- **Stock Tracking**: Automatic stock reduction when bills are created

## Project Structure

```
retail-billing-system/
├── pom.xml
├── README.md
├── src/
│   ├── main/
│   │   ├── java/com/retailshop/
│   │   │   ├── BillingSystemApplication.java
│   │   │   ├── entity/
│   │   │   │   ├── Product.java
│   │   │   │   ├── Bill.java
│   │   │   │   └── BillItem.java
│   │   │   ├── repository/
│   │   │   │   ├── ProductRepository.java
│   │   │   │   ├── BillRepository.java
│   │   │   │   └── BillItemRepository.java
│   │   │   ├── dto/
│   │   │   │   ├── BillItemDTO.java
│   │   │   │   └── CreateBillRequest.java
│   │   │   ├── service/
│   │   │   │   ├── ProductService.java
│   │   │   │   └── BillingService.java
│   │   │   └── controller/
│   │   │       ├── ProductController.java
│   │   │       └── BillingController.java
│   │   └── resources/
│   │       ├── application.properties
│   │       └── templates/
│   │           ├── index.html
│   │           ├── products.html
│   │           ├── add-product.html
│   │           ├── create-bill.html
│   │           ├── bill-list.html
│   │           ├── bill-view.html
│   │           └── sales-summary.html
```

## Tech Stack

- **Backend**: Spring Boot 3.3.0
- **Database**: MySQL
- **ORM**: JPA/Hibernate
- **Frontend**: Thymeleaf (Template Engine)
- **Build**: Maven
- **Java**: 17+

## Setup Instructions

### Prerequisites
- Java 17+
- MySQL 8.0+
- Maven 3.6+

### Database Setup

```sql
CREATE DATABASE billing_db;
USE billing_db;
```

### Configuration

Update `application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/billing_db
spring.datasource.username=root
spring.datasource.password=root
```

### Build and Run

```bash
mvn clean install
mvn spring-boot:run
```

Application will be available at: `http://localhost:8080`

## API Endpoints

### Products
- `GET /products` - View all products
- `GET /products/add` - Add product form
- `POST /products/save` - Save new product
- `GET /products/delete/{id}` - Delete product

### Billing
- `GET /billing` - Billing page
- `POST /billing/create` - Create new bill
- `GET /bills` - View all bills
- `GET /bills/{id}` - View specific bill
- `GET /sales-summary` - Today's sales summary

## Database Schema

### Products Table
```sql
CREATE TABLE products (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    category VARCHAR(100),
    price DOUBLE NOT NULL,
    stock INT NOT NULL
);
```

### Bills Table
```sql
CREATE TABLE bills (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    bill_date DATETIME NOT NULL,
    customer_name VARCHAR(255),
    subtotal DOUBLE NOT NULL,
    discount_percent DOUBLE DEFAULT 0,
    tax_percent DOUBLE DEFAULT 18,
    grand_total DOUBLE NOT NULL
);
```

### Bill Items Table
```sql
CREATE TABLE bill_items (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    bill_id BIGINT NOT NULL,
    product_id BIGINT NOT NULL,
    quantity INT NOT NULL,
    unit_price DOUBLE NOT NULL,
    line_total DOUBLE NOT NULL,
    FOREIGN KEY (bill_id) REFERENCES bills(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

## Sample Data

You can add sample products through the UI or use SQL:

```sql
INSERT INTO products (name, category, price, stock) VALUES
('Rice', 'Groceries', 50.00, 100),
('Sugar', 'Groceries', 40.00, 80),
('Oil', 'Groceries', 120.00, 50),
('Milk', 'Dairy', 60.00, 100),
('Bread', 'Bakery', 30.00, 50);
```

## Usage

1. **Add Products**: Go to Products menu, add items with name, category, price, and stock
2. **Create Bill**: Click on Billing, select products, set quantities, apply discount if needed
3. **View Bills**: Check all bills with date, customer name, and total amount
4. **Sales Report**: View today's total sales and number of bills

## Author
Developed as a Spring Boot learning project

## License
MIT
