# Retail Billing System - Complete Implementation Guide

## All Java Source Files

### 1. Bill.java Entity
```java
package com.retailshop.billing.entity;

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
    private LocalDateTime billDate = LocalDateTime.now();

    @Column(nullable = false)
    private Double totalAmount;

    private Double discountPercentage;
    private Double gstPercentage;

    @OneToMany(mappedBy = "bill", cascade = CascadeType.ALL, fetch = FetchType.EAGER)
    private List<BillItem> billItems;
}
```

### 2. BillItem.java Entity
```java
package com.retailshop.billing.entity;

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
    @JoinColumn(name = "product_id", nullable = false)
    private Product product;

    @Column(nullable = false)
    private Integer quantity;

    @Column(nullable = false)
    private Double lineTotal;

    @ManyToOne
    @JoinColumn(name = "bill_id", nullable = false)
    private Bill bill;
}
```

## Repository Interfaces

### ProductRepository
```java
package com.retailshop.billing.repository;
import org.springframework.data.jpa.repository.JpaRepository;
import com.retailshop.billing.entity.Product;
public interface ProductRepository extends JpaRepository<Product, Long> {}
```

### BillRepository
```java
package com.retailshop.billing.repository;
import org.springframework.data.jpa.repository.JpaRepository;
import com.retailshop.billing.entity.Bill;
import java.util.List;
public interface BillRepository extends JpaRepository<Bill, Long> {}
```

### BillItemRepository
```java
package com.retailshop.billing.repository;
import org.springframework.data.jpa.repository.JpaReposit

## Service Classes

### ProductService
```java
package com.retailshop.billing.service;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.retailshop.billing.entity.Product;
import com.retailshop.billing.repository.ProductRepository;
import java.util.List;

@Service
public class ProductService {
    @Autowired
    private ProductRepository productRepository;

    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    public Product getProductById(Long id) {
        return productRepository.findById(id).orElse(null);
    }

    public Product saveProduct(Product product) {
        return productRepository.save(product);
    }

    public void deleteProduct(Long id) {
        productRepository.deleteById(id);
    }
}
```

### BillingService
```java
package com.retailshop.billing.service;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.retailshop.billing.entity.Bill;
import com.retailshop.billing.entity.BillItem;
import com.retailshop.billing.entity.Product;
import com.retailshop.billing.repository.BillRepository;
import com.retailshop.billing.repository.BillItemRepository;
import java.time.LocalDateTime;
import java.util.List;

@Service
public class BillingService {
    @Autowired
    private BillRepository billRepository;

    @Autowired
    private BillItemRepository billItemRepository;

    public Bill createBill(Bill bill, List<BillItem> items) {
        double total = 0;
        Bill savedBill = billRepository.save(bill);
        
        for (BillItem item : items) {
            item.setBill(savedBill);
            total += item.getLineTotal();
            billItemRepository.save(item);
        }
        
        savedBill.setTotalAmount(total);
        return billRepository.save(savedBill);
    }

    public List<Bill> getAllBills() {
        return billRepository.findAll();
    }

    public Bill getBillById(Long id) {
        return billRepository.findById(id).orElse(null);
    }
}
```
ory;
import com.retailshop.billing.entity.BillItem;
public interface BillItemRepository extends JpaRepository<BillItem, Long> {}
```


## Controller Classes

### ProductController
```java
package com.retailshop.billing.controller;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import com.retailshop.billing.entity.Product;
import com.retailshop.billing.service.ProductService;

@Controller
@RequestMapping("/products")
public class ProductController {
    @Autowired
    private ProductService productService;

    @GetMapping
    public String listProducts(Model model) {
        model.addAttribute("products", productService.getAllProducts());
        return "products";
    }

    @GetMapping("/new")
    public String newProductForm(Model model) {
        model.addAttribute("product", new Product());
        return "product-form";
    }

    @PostMapping("/save")
    public String saveProduct(@ModelAttribute Product product) {
        productService.saveProduct(product);
        return "redirect:/products";
    }

    @GetMapping("/edit/{id}")
    public String editProduct(@PathVariable Long id, Model model) {
        model.addAttribute("product", productService.getProductById(id));
        return "product-form";
    }

    @GetMapping("/delete/{id}")
    public String deleteProduct(@PathVariable Long id) {
        productService.deleteProduct(id);
        return "redirect:/products";
    }
}
```

### BillingController
```java
package com.retailshop.billing.controller;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import com.retailshop.billing.entity.Bill;
import com.retailshop.billing.service.BillingService;
import com.retailshop.billing.service.ProductService;

@Controller
@RequestMapping("/billing")
public class BillingController {
    @Autowired
    private BillingService billingService;
    @Autowired
    private ProductService productService;

    @GetMapping("/create")
    public String createBillForm(Model model) {
        model.addAttribute("products", productService.getAllProducts());
        return "create-bill";
    }

    @GetMapping("/bills")
    public String listBills(Model model) {
        model.addAttribute("bills", billingService.getAllBills());
        return "bills";
    }

    @GetMapping("/summary")
    public String salesSummary(Model model) {
        return "sales-summary";
    }
}
```
