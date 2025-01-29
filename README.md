# G2Bulk Public API Documentation

Welcome to the G2Bulk Public API documentation. This guide provides comprehensive details about the API, including available endpoints, authentication methods, request and response formats, and error handling.

## Base URL
```
https://api.g2bulk.com/v1/
```

## Table of Contents
- [Authentication](#authentication)
- [Public Endpoints](#public-endpoints)
  - [Product Endpoints](#product-endpoints)
  - [Category Endpoints](#category-endpoints)
- [Protected Endpoints](#protected-endpoints)
  - [User Endpoints](#user-endpoints)
  - [Admin Endpoints](#admin-endpoints)
- [Rate Limiting](#rate-limiting)
- [Error Handling](#error-handling)
- [Support](#support)

---

## Authentication

### Public Access 
- **Header:** `X-API-Key`
- **Description:** Some endpoints require an API key for access. You can obtain an API key from the G2Bulk Telegram bot.

### Admin Access 
- **Header:** `Authorization: Bearer <JWT_TOKEN>`
- **Description:** Endpoints requiring admin privileges use JWT-based authentication. Obtain the token by logging in as an admin.

---

## Public Endpoints

### Product Endpoints

#### Get All Products
- **Endpoint:** `GET /v1/product`
- **Description:** Retrieve a list of all available products.
- **Authentication:** None
- **Response:** JSON list of products

#### Get Product by ID
- **Endpoint:** `GET /v1/product/:id`
- **Description:** Retrieve details of a specific product using its ID.
- **Authentication:** None
- **Response:** JSON object containing product details

#### Purchase Product
- **Endpoint:** `POST /v1/product/:id/purchase`
- **Description:** Purchase a product using its ID.
- **Authentication:** `X-API-Key`
- **Request Body:**
  ```json
  {
    "quantity": <number>
  }
  ```
- **Response:** Purchase confirmation

### Category Endpoints

#### Get All Categories
- **Endpoint:** `GET /v1/category`
- **Description:** Retrieve a list of all product categories.
- **Authentication:** None
- **Response:** JSON list of categories

#### Get Category by ID
- **Endpoint:** `GET /v1/category/:id`
- **Description:** Retrieve details of a specific category.
- **Authentication:** None
- **Response:** JSON object containing category details

#### Create Category
- **Endpoint:** `POST /v1/category`
- **Description:** Create a new product category.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** JSON object of the created category

---

## Protected Endpoints

### User Endpoints

#### Get All Users
- **Endpoint:** `GET /v1/user`
- **Description:** Retrieve a list of all users.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** JSON list of users

#### Get User by ID
- **Endpoint:** `GET /v1/user/:id`
- **Description:** Retrieve details of a specific user.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** JSON object containing user details

#### Create User
- **Endpoint:** `POST /v1/user`
- **Description:** Create a new user.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** JSON object of the created user

#### Update User
- **Endpoint:** `PUT /v1/user/:id`
- **Description:** Update details of a specific user.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** JSON object with updated user details

#### Delete User
- **Endpoint:** `DELETE /v1/user/:id`
- **Description:** Delete a specific user.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** Deletion confirmation

### Admin Endpoints

#### Change Admin Password
- **Endpoint:** `PATCH /v1/admin/changePassword`
- **Description:** Change the admin's password.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** Password change confirmation

#### Add Balance
- **Endpoint:** `POST /v1/admin/addBalance`
- **Description:** Add balance to the user account.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** Balance update confirmation

#### Admin Login
- **Endpoint:** `POST /v1/admin/login`
- **Description:** Login as an admin to obtain a JWT token.
- **Authentication:** None
- **Response:** JWT token

#### Add Item to Inventory
- **Endpoint:** `POST /v1/admin/inventory`
- **Description:** Add an item to the inventory.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** Inventory update confirmation

#### Bulk Insert to Inventory
- **Endpoint:** `POST /v1/admin/bulkInsert`
- **Description:** Bulk insert multiple items into the inventory.
- **Authentication:** `Authorization: Bearer <JWT_TOKEN>`
- **Response:** Bulk insert confirmation

---

## Rate Limiting
- **Limit:** `300 requests per 10 seconds`
- **Warning** `Multiple failed attempts to admin endpoint will trigger a permanent IP ban. (Protected by Fail2Ban)`
- **Description:** To ensure fair usage and prevent abuse, requests exceeding this limit will receive a `429 Too Many Requests` response.

---

## Error Handling

| Status Code | Meaning |
|-------------|---------|
| **400** | Bad Request - Invalid request format |
| **401** | Unauthorized - Authentication failed or missing credentials |
| **404** | Not Found - Requested resource does not exist |
| **429** | Too Many Requests - Rate limit exceeded |
| **500** | Internal Server Error - An unexpected server error occurred |

---

## Support
For any questions, issues, or API key requests, please contact our [support team](https://t.me/seedon).

---

This documentation is subject to updates. Always check for the latest version to ensure compatibility with the API.

