# Thymeleaf HTML Templates for Retail Billing System

## 1. products.html - Product List Page

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Products List</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 12px; text-align: left; }
        th { background-color: #4CAF50; color: white; }
        .btn { padding: 8px 16px; margin: 5px; text-decoration: none; border-radius: 4px; }
        .btn-primary { background-color: #007bff; color: white; }
        .btn-danger { background-color: #dc3545; color: white; }
        .btn-warning { background-color: #ffc107; color: black; }
    </style>
</head>
<body>
    <h1>Product Management</h1>
    <a href="/products/new" class="btn btn-primary">Add New Product</a>
    <table>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Price</th>
            <th>Stock</th>
            <th>Actions</th>
        </tr>
        <tr th:each="product : ${products}">
            <td th:text="${product.id}"></td>
            <td th:text="${product.name}"></td>
            <td>₹<span th:text="${product.price}"></span></td>
            <td th:text="${product.stock}"></td>
            <td>
                <a th:href="@{/products/edit/{id}(id=${product.id})}" class="btn btn-warning">Edit</a>
                <a th:href="@{/products/delete/{id}(id=${product.id})}" class="btn btn-danger"
                   onclick="return confirm('Are you sure?')">Delete</a>
            </td>
        </tr>
    </table>
</body>
</html>
```

## 2. product-form.html - Add/Edit Product Form

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Product Form</title>
    <style>
        body { font-family: Arial; margin: 20px; }
        form { max-width: 500px; }
        input, textarea { width: 100%; padding: 10px; margin: 10px 0; box-sizing: border-box; }
        button { padding: 10px 20px; background-color: #4CAF50; color: white; border: none; cursor: pointer; }
    </style>
</head>
<body>
    <h1>Product Form</h1>
    <form action="/products/save" method="post">
        <input type="hidden" th:field="${product.id}" />
        
        <label>Product Name</label>
        <input type="text" th:field="${product.name}" required />
        
        <label>Price</label>
        <input type="number" th:field="${product.price}" step="0.01" required />
        
        <label>Stock</label>
        <input type="number" th:field="${product.stock}" required />
        
        <label>Description</label>
        <textarea th:field="${product.description}"></textarea>
        
        <button type="submit">Save Product</button>
        <a href="/products">Cancel</a>
    </form>
</body>
</html>
```

## 3. create-bill.html - Create Bill Page

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Create Bill</title>
    <style>
        body { font-family: Arial; margin: 20px; }
        .container { max-width: 900px; }
        table { width: 100%; border-collapse: collapse; margin: 20px 0; }
        th, td { border: 1px solid #ddd; padding: 12px; }
        .total-section { background: #f0f0f0; padding: 15px; margin: 20px 0; }
        .btn { padding: 10px 20px; margin: 5px; }
    </style>
    <script>
        function addProduct(productId, productName, price) {
            var table = document.getElementById('billItems');
            var row = table.insertRow(-1);
            row.innerHTML = '<td>' + productName + '</td><td><input type="number" value="1" min="1"></td><td>₹' + price + '</td><td>' + (price * 1) + '</td>';
        }
    </script>
</head>
<body>
    <h1>Create New Bill</h1>
    <div class="container">
        <h3>Select Products</h3>
        <div th:each="product : ${products}">
            <button th:text="${product.name} + ' - ₹' + ${product.price}"
                    th:onclick="|addProduct(${product.id}, '${product.name}', ${product.price})|" class="btn"></button>
        </div>
        
        <h3>Bill Items</h3>
        <table id="billItems">
            <tr><th>Product</th><th>Quantity</th><th>Unit Price</th><th>Total</th></tr>
        </table>
        
        <div class="total-section">
            <h4>Subtotal: ₹<span id="subtotal">0</span></h4>
            <h4>Discount (%): <input type="number" id="discount" value="0" /> </h4>
            <h4>GST (18%): ₹<span id="gst">0</span></h4>
            <h3>Grand Total: ₹<span id="grandTotal">0</span></h3>
            <button class="btn" style="background-color: #4CAF50; color: white;">Generate Bill</button>
        </div>
    </div>
</body>
</html>
```

## 4. bills.html - Bill History Page

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Bill History</title>
    <style>
        body { font-family: Arial; margin: 20px; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 12px; text-align: left; }
        th { background-color: #4CAF50; color: white; }
    </style>
</head>
<body>
    <h1>Bill History</h1>
    <table>
        <tr><th>Bill ID</th><th>Date</th><th>Total Amount</th><th>Discount</th><th>GST</th></tr>
        <tr th:each="bill : ${bills}">
            <td th:text="${bill.id}"></td>
            <td th:text="${bill.billDate}"></td>
            <td>₹<span th:text="${bill.totalAmount}"></span></td>
            <td th:text="${bill.discountPercentage} + '%'"></td>
            <td th:text="${bill.gstPercentage} + '%'"></td>
        </tr>
    </table>
</body>
</html>
```

## 5. sales-summary.html - Sales Summary Page

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Sales Summary</title>
    <style>
        body { font-family: Arial; margin: 20px; }
        .card { background: #f9f9f9; padding: 20px; margin: 10px 0; border-radius: 5px; }
        h2 { color: #4CAF50; }
    </style>
</head>
<body>
    <h1>Sales Summary</h1>
    <div class="card">
        <h2>Today's Sales</h2>
        <p>Total Bills: <strong th:text="${#lists.size(bills)}"></strong></p>
        <p>Total Revenue: <strong>₹<span th:text="${#aggregates.sum(bills.![totalAmount])}"></span></strong></p>
    </div>
</body>
</html>
```

