# Online Bookstore Database Design

This document describes the **initial database structure** for the Online Bookstore project, including tables, fields, relationships, and additional enhancements for handling **online reading, product types, categories, and tags**.

---

## Tables & Schema

### 1. Permissions Table
Stores granular permissions for system actions.

| Column       | Type        | Notes                         |
|--------------|------------|-------------------------------|
| id           | INT (PK)   | Auto-increment                |
| name         | VARCHAR    | Unique permission identifier  |
| description  | TEXT       | Optional description          |

---

### 2. Roles Table
Defines roles such as `admin`, `seller`, `client`.

| Column       | Type        | Notes                         |
|--------------|------------|-------------------------------|
| id           | INT (PK)   | Auto-increment                |
| name         | VARCHAR    | Unique                        |
| description  | TEXT       | Optional                      |

---

### 3. Role-Permission Table
Many-to-many relationship between roles and permissions.

| Column       | Type      | Notes                         |
|--------------|-----------|-------------------------------|
| id           | INT (PK) | Auto-increment                |
| role_id      | INT (FK) | References `roles.id`         |
| permission_id| INT (FK) | References `permissions.id`   |

---

### 4. Users Table
Stores basic user information and credentials.

| Column       | Type          | Notes                                 |
|--------------|---------------|---------------------------------------|
| id           | INT (PK)      | Auto-increment                        |
| name         | VARCHAR       | Full name                              |
| email        | VARCHAR       | Unique                                 |
| username     | VARCHAR       | Unique                                 |
| password     | VARCHAR       | Hashed                                 |
| profile_pic  | VARCHAR       | URL/path to image                       |
| type         | ENUM          | ["admin", "seller", "client"]          |
| created_at   | TIMESTAMP     | Default current timestamp              |
| updated_at   | TIMESTAMP     | On update current timestamp            |

---

### 5. User Details Table
Optional extra information about users.

| Column       | Type        | Notes                       |
|--------------|------------|-----------------------------|
| id           | INT (PK)  | Auto-increment              |
| user_id      | INT (FK)  | References `users.id`      |
| address      | TEXT       |                               |
| phone        | VARCHAR    |                               |
| dob          | DATE       | Date of birth                |
| age          | INT        | Calculated or manual         |

---

### 6. User-Role Table
Many-to-many relationship: users can have multiple roles.

| Column       | Type      | Notes                         |
|--------------|-----------|-------------------------------|
| id           | INT (PK) | Auto-increment                |
| user_id      | INT (FK) | References `users.id`         |
| role_id      | INT (FK) | References `roles.id`         |

---

### 7. Seller Information Table
Extra data for seller users.

| Column          | Type      | Notes                    |
|-----------------|-----------|--------------------------|
| id              | INT (PK) | Auto-increment           |
| user_id         | INT (FK) | References `users.id`    |
| business_name   | VARCHAR   |                          |
| business_image  | VARCHAR   | URL/path                 |
| extra_info      | JSON      | Flexible for future data |

---

### 8. Categories Table
Each book can belong to one or multiple categories.

| Column   | Type      | Notes                  |
|----------|-----------|------------------------|
| id       | INT (PK) | Auto-increment         |
| name     | VARCHAR   | Unique category name    |
| description | TEXT   | Optional               |

---

### 9. Tags Table
Optional tags for books.

| Column   | Type      | Notes                 |
|----------|-----------|-----------------------|
| id       | INT (PK) | Auto-increment        |
| name     | VARCHAR   | Unique tag name       |

---

### 10. Product Table (Books)
Includes fields for online reading, saleable, and product type.

| Column             | Type        | Notes                                      |
|--------------------|------------|--------------------------------------------|
| id                 | INT (PK)  | Auto-increment                             |
| sku                | VARCHAR    | Unique, not null                           |
| name               | VARCHAR    | Book title                                 |
| short_description  | TEXT       |                                            |
| full_description   | TEXT       |                                            |
| price              | DECIMAL    |                                            |
| on_sale            | ENUM       | ["percentage", "fixed", "none"]           |
| sale_price         | DECIMAL    | Calculated based on `on_sale`             |
| rating             | DECIMAL    | Average rating                             |
| stock              | INT        | For physical books                          |
| product_type       | ENUM       | ["ebook", "physical", "audiobook"]        |
| saleable           | BOOLEAN    | Can be purchased?                           |
| online_readable    | BOOLEAN    | Can be read online?                         |
| language           | VARCHAR    | e.g., English, French                        |
| edition            | VARCHAR    | Optional                                    |
| isbn               | VARCHAR    | Optional                                    |
| cover_image        | VARCHAR    | URL/path                                     |
| created_at         | TIMESTAMP  |                                            |
| updated_at         | TIMESTAMP  |                                            |

---

### 11. Product-Category Table
Many-to-many relationship.

| Column      | Type      | Notes                    |
|-------------|-----------|--------------------------|
| id          | INT (PK) | Auto-increment           |
| product_id  | INT (FK) | References `products.id` |
| category_id | INT (FK) | References `categories.id` |

---

### 12. Product-Tag Table
Many-to-many relationship.

| Column    | Type      | Notes                    |
|-----------|-----------|--------------------------|
| id        | INT (PK) | Auto-increment           |
| product_id| INT (FK) | References `products.id` |
| tag_id    | INT (FK) | References `tags.id`     |

---

### 13. Product Meta Table
Flexible key-value data for products.

| Column | Type    | Notes                     |
|--------|--------|---------------------------|
| id     | INT    | PK                        |
| product_id | INT | FK → `products.id`        |
| key    | VARCHAR | e.g., "delivery", "language" |
| value  | TEXT    | e.g., "2-3 days", "English" |

---

### 14. Product Comments Table
User reviews and optional images.

| Column    | Type     | Notes                   |
|-----------|---------|------------------------|
| id        | INT     | PK                     |
| product_id | INT    | FK → `products.id`     |
| user_id   | INT     | FK → `users.id`        |
| comment   | TEXT    |                        |
| images    | JSON    | Optional multiple images|
| rating    | DECIMAL | Optional user rating    |
| created_at| TIMESTAMP |                        |

---

### 15. Seller Products Table
Many-to-many relationship: sellers can sell multiple products.

| Column      | Type     | Notes                      |
|-------------|----------|----------------------------|
| id          | INT      | PK                         |
| product_id  | INT      | FK → `products.id`         |
| seller_id   | INT      | FK → `users.id` (type=seller) |

---

### 16. Seller Coupons Table
Discounts offered by sellers.

| Column       | Type      | Notes                           |
|--------------|-----------|---------------------------------|
| id           | INT (PK) | Auto-increment                  |
| name         | VARCHAR   | Coupon code or name             |
| type         | ENUM      | ["%", "fixed"]                  |
| discount     | DECIMAL   | Amount or percentage            |
| allowed_products | JSON  | List of product_ids             |
| status       | ENUM      | ["active", "inactive"]             |
| start_date   | DATE      | Optional: when coupon becomes active |
| end_date     | DATE      | Optional: when coupon expires    |

---

### 17. Orders Table
Tracks client purchases.

| Column           | Type        | Notes                               |
|------------------|------------|-------------------------------------|
| id               | INT (PK)  | Auto-increment                      |
| product_id       | INT (FK)  | References `products.id`            |
| seller_id        | INT (FK)  | References `users.id` (seller)     |
| client_id        | INT (FK)  | References `users.id` (client)     |
| delivery_date    | DATE       | Optional                             |
| quantity         | INT        |                                     |
| actual_price     | DECIMAL    | Price before discount                |
| coupon_id        | INT (FK)  | References `seller_coupons.id`      |
| discounted_price | DECIMAL    | Final price after discount           |
| total_price | DECIMAL    | total price after delivery price + discount           |
| delivery_address | TEXT       |                                     |
| payment_type     | ENUM       | ["bank", "cash on delivery", "stripe"] |
| status           | ENUM       | ["pending", "completed", "canceled", "shipped"] |
| created_at       | TIMESTAMP  |                                     |
| updated_at       | TIMESTAMP  |                                     |

---

## Relationships Overview
- **Users ↔ Roles** → Many-to-Many (`user_role`)  
- **Roles ↔ Permissions** → Many-to-Many (`role_permission`)  
- **Sellers ↔ Products** → Many-to-Many (`seller_products`)  
- **Products ↔ Product Meta** → One-to-Many  
- **Products ↔ Product Comments** → One-to-Many  
- **Products ↔ Categories** → Many-to-Many  
- **Products ↔ Tags** → Many-to-Many  
- **Orders ↔ Products, Users, Coupons** → One-to-Many  

---

## Notes & Enhancements
- Supports **online reading**, **physical sales**, and **audiobooks**.  
- Flexible **role-based permissions** for admin, employee, seller, client.  
- Supports **categories and tags** for filtering and recommendations.  
- Stores **book metadata** (language, edition, ISBN, cover).  
- Supports **digital downloads** or physical delivery.  

---
