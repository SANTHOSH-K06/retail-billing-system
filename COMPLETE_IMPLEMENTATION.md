# Complete Retail Billing System Implementation

## Complete Source Code for All Files

Copy these files into your project in the correct directories.

---

## File: src/main/java/com/retailshop/BillingSystemApplication.java

```java
package com.retailshop;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class BillingSystemApplication {
    public static void main(String[] args) {
        SpringApplication.run(BillingSystemApplication.class, args);
    }
}
```

---

## File: src/main/java/com/retailshop/entity/Product.java

```java
package com.retailshop.entity;

import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Table(name = "products")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false)
    private String category;
    
    @Column(nullable = false)
    private Double price;
    
    @Column(nullable = false)
    private Integer stock;
}
```

---

## File: src/main/java/com/retailshop/entity/Bill.java

```java
package com.retailshop.entity;

import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.time.LocalDateTime;
import java.util.List;

@Entity
@Table(name = "bills")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Bill {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private LocalDateTime billDate;
    
    private String customerName;
    
    @Column(nullable = false)
    private Double subtotal;
    
    @Column(nullable = false)
    private Double discountPercent;
    
    @Column(nullable = false)
    private Double taxPercent;
    
    @Column(nullable = false)
    private Double grandTotal;
    
    @OneToMany(mappedBy = "bill", cascade = CascadeType.ALL, fetch = FetchType.EAGER)
    private List<BillItem> billItems;
}
```

---

## File: src/main/java/com/retailshop/entity/BillItem.java

```java
package com.retailshop.entity;

import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Table(name = "bill_items")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class BillItem {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "bill_id", nullable = false)
    private Bill bill;
    
    @ManyToOne
    @JoinColumn(name = "product_id", nullable = false)
    private Product product;
    
    @Column(nullable = false)
    private Integer quantity;
    
    @Column(nullable = false)
    private Double unitPrice;
    
    @Column(nullable = false)
    private Double lineTotal;
}
```

---

## File: src/main/java/com/retailshop/repository/ProductRepository.java

```java
package com.retailshop.repository;

import com.retailshop.entity.Product;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
}
```

---

## File: src/main/java/com/retailshop/repository/BillRepository.java

```java
package com.retailshop.repository;

import com.retailshop.entity.Bill;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.time.LocalDateTime;
import java.util.List;

@Repository
public interface BillRepository extends JpaRepository<Bill, Long> {
    List<Bill> findByBillDateBetween(LocalDateTime start, LocalDateTime end);
}
```

---

## File: src/main/java/com/retailshop/repository/BillItemRepository.java

```java
package com.retailshop.repository;

import com.retailshop.entity.BillItem;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface BillItemRepository extends JpaRepository<BillItem, Long> {
}
```

---

## File: src/main/java/com/retailshop/dto/BillItemDTO.java

```java
package com.retailshop.dto;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class BillItemDTO {
    private Long productId;
    private Integer quantity;
}
```

---

## File: src/main/java/com/retailshop/dto/CreateBillRequest.java

```java
package com.retailshop.dto;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.util.List;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class CreateBillRequest {
    private String customerName;
    private List<BillItemDTO> items;
    private Double discountPercent;
    private Double taxPercent;
}
```

(Continued in next part...)
