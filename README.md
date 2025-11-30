# G2Bulk Public API Documentation

Professional gaming top-up and digital voucher services. Secure, reliable, and lightning-fast API designed for developers and businesses. Support for PUBG Mobile, Mobile Legends, Free Fire, and more.

## Base URL
```
https://api.g2bulk.com/v1/
```

**Last Updated:** 19 November 2025 12:00 PM UTC

## Quick Stats

- ⚡ **99.9%** Uptime
- 🔒 **Secure** API Authentication
- 🎮 **10+** Games Supported
- 🚀 **<200ms** Avg Response Time

## Table of Contents

- [Authentication](#authentication)
- [User Endpoints](#user-endpoints)
- [Products & Categories](#products--categories)
- [Orders](#orders)
- [Games & Top-up Services](#games--top-up-services)
- [Transactions](#transactions)
- [Plans & Subscriptions](#plans--subscriptions)
- [Rate Limiting](#rate-limiting)
- [Error Handling](#error-handling)
- [Support](#support)

---

## Authentication

### API Key Authentication

Protected endpoints require authentication using an API key. Include your API key in the request header for all authenticated requests.

**Request Header:**
```
X-API-Key: your_api_key_here
```

**Get Your API Key:**
Obtain an API key instantly from our [Telegram Bot](https://t.me/G2BULKBOT)

**⚠️ Security Warning:**
Multiple failed authentication attempts will trigger a permanent IP ban. Keep your API key secure and never share it publicly.

---

## User Endpoints

### Get Me

Retrieve authenticated user details including balance, username, and user ID.

- **Endpoint:** `GET /v1/getMe`
- **Authentication:** Required
- **Response:**
  ```json
  {
    "success": true,
    "user_id": 123456789,
    "username": "johndoe",
    "first_name": "John Doe",
    "balance": 8.74
  }
  ```

---

## Products & Categories

### Get Categories

Retrieve all available product categories with product counts.

- **Endpoint:** `GET /v1/category`
- **Authentication:** Public
- **Response:**
  ```json
  {
    "success": true,
    "categories": [
      {
        "id": 1,
        "title": "PUBG Mobile UC Vouchers",
        "description": "",
        "product_count": 11
      },
      {
        "id": 2,
        "title": "Razer Gold Accounts",
        "description": "",
        "product_count": 5
      }
    ]
  }
  ```

### Get Products

Retrieve all available products with pricing and stock information.

- **Endpoint:** `GET /v1/products`
- **Authentication:** Public
- **Response:**
  ```json
  {
    "success": true,
    "products": [
      {
        "id": 1,
        "title": "60 UC Voucher",
        "description": "",
        "category_id": 1,
        "category_title": "PUBG Mobile UC Vouchers",
        "unit_price": 0.84,
        "stock": 1006
      }
    ]
  }
  ```

### Get Product by ID

Retrieve detailed information about a specific product.

- **Endpoint:** `GET /v1/products/:id`
- **Authentication:** Public

### Get Category Products

Retrieve all products from a specific category.

- **Endpoint:** `GET /v1/category/:id`
- **Authentication:** Public

### Purchase Product

Purchase a product with specified quantity. Balance is automatically deducted.

- **Endpoint:** `POST /v1/products/:id/purchase`
- **Authentication:** Required
- **Request Body:**
  ```json
  {
    "quantity": 5
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "order_id": 123,
    "transaction_id": 456,
    "product_id": 1,
    "product_title": "60 UC Voucher",
    "delivery_items": ["KEY1", "KEY2", "KEY3"]
  }
  ```

---

## Orders

### Get All Orders

Retrieve complete order history for the authenticated user.

- **Endpoint:** `GET /v1/orders`
- **Authentication:** Required
- **Response:**
  ```json
  {
    "success": true,
    "orders": [
      {
        "id": 1,
        "user_id": 123456789,
        "product_id": 1,
        "product_title": "60 UC Voucher",
        "quantity": 1,
        "total_price": 0.84,
        "status": "COMPLETED",
        "description": "Purchase from G2Bulk Bot"
      }
    ]
  }
  ```

### Get Order by ID

Retrieve detailed information about a specific order.

- **Endpoint:** `GET /v1/orders/:id`
- **Authentication:** Required

---

## Games & Top-up Services

### Get Available Games

Retrieve all supported games for top-up services.

- **Endpoint:** `GET /v1/games`
- **Authentication:** Public
- **Response:**
  ```json
  {
    "success": true,
    "games": [
      {
        "id": 1,
        "code": "pubg_mobile",
        "name": "PUBG Mobile",
        "image_url": "/images/pubg_mobile.png"
      },
      {
        "id": 2,
        "code": "free_fire",
        "name": "Free Fire",
        "image_url": "/images/free_fire.png"
      }
    ]
  }
  ```

### Get Game Fields

Get required input fields for a specific game (e.g., user ID, server ID).

- **Endpoint:** `POST /v1/games/fields`
- **Authentication:** Public
- **Request Body:**
  ```json
  {
    "game": "mlbb"
  }
  ```
- **Response:**
  ```json
  {
    "code": "200",
    "info": {
      "fields": ["userid", "serverid"],
      "notes": "Not available for Indonesia users"
    }
  }
  ```

### Check Player ID

Validate a player ID before placing an order. Returns player name if valid.

- **Endpoint:** `POST /v1/games/checkPlayerId`
- **Authentication:** Public
- **Request Body:**
  ```json
  {
    "game": "mlbb",
    "user_id": "123456789",
    "server_id": "2001"
  }
  ```
- **Response (Valid):**
  ```json
  {
    "valid": "valid",
    "name": "John Doe",
    "openid": "41581795132966184"
  }
  ```

### Get Game Catalogue

Get all available denominations/packages for a specific game with current prices.

- **Endpoint:** `GET /v1/games/:code/catalogue`
- **Authentication:** Public
- **Response:**
  ```json
  {
    "success": true,
    "game": {
      "code": "pubgm",
      "name": "PUBG Mobile",
      "image_url": "/images/pubgm.png"
    },
    "catalogues": [
      {
        "id": 1,
        "name": "60 UC",
        "amount": 0.88
      },
      {
        "id": 2,
        "name": "120 UC",
        "amount": 1.75
      }
    ]
  }
  ```

**ℹ️ Note:** Prices are automatically updated every 5 minutes to ensure accuracy.

### Create Game Order

Place a game top-up order. Player ID is validated automatically and balance is checked before processing.

- **Endpoint:** `POST /v1/games/:code/order`
- **Authentication:** Required
- **Request Body:**
  ```json
  {
    "catalogue_name": "60 UC",
    "player_id": "5679523421",
    "server_id": "2001",
    "remark": "Optional note",
    "callback_url": "https://your-domain.com/webhook/order-status"
  }
  ```
- **Response (Success):**
  ```json
  {
    "success": true,
    "message": "Order created successfully. We are processing your order.",
    "order": {
      "order_id": 42,
      "game": "PUBG Mobile",
      "catalogue": "60 UC",
      "player_id": "5679523421",
      "player_name": "PlayerName",
      "price": 0.88,
      "status": "PENDING",
      "callback_url": "https://your-domain.com/webhook/order-status"
    }
  }
  ```

#### Callback URL (Optional)

You can provide an optional `callback_url` to receive automatic notifications when the order status changes. This is particularly useful for automating your workflow without polling for order status.

**Callback Properties:**

| Property | Details |
|----------|---------|
| **Method** | POST request with JSON body |
| **Timeout** | 10 seconds |
| **Retry Policy** | Single retry on timeout or failure |
| **When Called** | Whenever the order status changes (PENDING → PROCESSING → COMPLETED/FAILED) |

**Callback Payload:**

Your callback URL will receive a POST request with the following JSON structure:

```json
{
  "order_id": 42,
  "game_code": "pubgm",
  "game_name": "PUBG Mobile",
  "player_id": "5679523421",
  "player_name": "PlayerName",
  "server_id": "2001",
  "denom_id": "60 UC",
  "price": 0.88,
  "status": "COMPLETED",
  "message": "Order completed successfully",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**ℹ️ Important:** Your callback endpoint should respond with HTTP status 200-299 within 10 seconds. Failed callbacks will be retried once. Ensure your endpoint can handle duplicate notifications gracefully.

#### Order Status Values

| Status | Description |
|--------|-------------|
| `PENDING` | Order created and waiting to be processed |
| `PROCESSING` | Order is currently being processed |
| `COMPLETED` | Order completed successfully, items delivered |
| `FAILED` | Order failed, balance automatically refunded |

### Check Order Status

Check the current status of a specific game order.

- **Endpoint:** `POST /v1/games/order/status`
- **Authentication:** Required
- **Request Body:**
  ```json
  {
    "order_id": 42,
    "game": "pubgm"
  }
  ```

### Get All Game Orders

Retrieve complete game top-up order history for the authenticated user.

- **Endpoint:** `GET /v1/games/orders`
- **Authentication:** Required

### Legacy PUBG Mobile Endpoints

**⚠️ Deprecated:** These endpoints are maintained for backward compatibility. Please use the unified `/v1/games/*` endpoints for new integrations.

- `GET /v1/topup/pubgMobile/offers` - Get available PUBG Mobile top-up offers (Public)
- `POST /v1/topup/pubgMobile/checkPlayerId` - Validate PUBG Mobile player ID (Public)
- `POST /v1/topup/pubgMobile/purchase` - Purchase PUBG Mobile top-up (Authentication Required)
- `GET /v1/topup/pubgMobile/status` - Check PUBG Mobile top-up status (Authentication Required)
- `GET /v1/topups` - Get all PUBG Mobile top-up history (Authentication Required)

---

## Transactions

### Get Transactions

Retrieve complete transaction history including balance additions, charges, and refunds.

- **Endpoint:** `GET /v1/transactions`
- **Authentication:** Required
- **Response:**
  ```json
  {
    "success": true,
    "data": [
      {
        "id": 45,
        "user_id": 123456789,
        "transaction_type": "charge_balance",
        "amount": "1.126",
        "balance_before": "10.000",
        "balance_after": "8.874",
        "status": "success",
        "description": "Purchase PUBG Mobile 60 UC",
        "created_at": "2025-10-19T05:30:00Z"
      }
    ]
  }
  ```

### Transaction Types

| Type | Description |
|------|-------------|
| `add_balance` | Balance added (manual addition or refund) |
| `charge_balance` | Balance deducted (purchase or top-up) |

---

## Plans & Subscriptions

Users can subscribe to recurring plans. Balance is deducted when creating a subscription, and only one active subscription is allowed at a time.

### Get All Plans

Retrieve all available subscription plans with pricing and duration details.

- **Endpoint:** `GET /v1/plans`
- **Authentication:** Public
- **Response:**
  ```json
  {
    "success": true,
    "plans": [
      {
        "ID": 1,
        "CreatedAt": "2025-11-27T10:00:00Z",
        "UpdatedAt": "2025-11-27T10:00:00Z",
        "DeletedAt": null,
        "name": "Basic Plan",
        "description": "Basic subscription with standard features",
        "price": 9.99,
        "duration_days": 30
      },
      {
        "ID": 2,
        "CreatedAt": "2025-11-27T10:00:00Z",
        "UpdatedAt": "2025-11-27T10:00:00Z",
        "DeletedAt": null,
        "name": "Premium Plan",
        "description": "Premium subscription with all features",
        "price": 29.99,
        "duration_days": 30
      }
    ]
  }
  ```

### Get Plan by ID

Retrieve detailed information about a specific subscription plan.

- **Endpoint:** `GET /v1/plans/:id`
- **Authentication:** Public
- **Response:**
  ```json
  {
    "success": true,
    "plan": {
      "ID": 1,
      "CreatedAt": "2025-11-27T10:00:00Z",
      "UpdatedAt": "2025-11-27T10:00:00Z",
      "DeletedAt": null,
      "name": "Basic Plan",
      "description": "Basic subscription with standard features",
      "price": 9.99,
      "duration_days": 30
    }
  }
  ```

### Create Subscription

Create a subscription for the authenticated user. Plan price is automatically deducted.

- **Endpoint:** `POST /v1/subscriptions`
- **Authentication:** Required
- **Request Body:**
  ```json
  {
    "plan_id": 1
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "subscription": {
      "ID": 1,
      "CreatedAt": "2025-11-27T12:00:00Z",
      "UpdatedAt": "2025-11-27T12:00:00Z",
      "DeletedAt": null,
      "user_id": 123456,
      "plan_id": 1,
      "start_date": "2025-11-27T12:00:00Z",
      "end_date": "2025-12-27T12:00:00Z",
      "status": "active",
      "plan": {
        "ID": 1,
        "name": "Basic Plan",
        "description": "Basic subscription with standard features",
        "price": 9.99,
        "duration_days": 30
      }
    }
  }
  ```

**⚠️ Requirements:**

- User must have sufficient balance
- Only one active subscription per user
- A transaction record is automatically created

### Get User Subscriptions

Retrieve all subscriptions (past and present) for the authenticated user.

- **Endpoint:** `GET /v1/subscriptions`
- **Authentication:** Required
- **Response:**
  ```json
  {
    "success": true,
    "subscriptions": [
      {
        "ID": 1,
        "CreatedAt": "2025-11-27T12:00:00Z",
        "UpdatedAt": "2025-11-27T12:00:00Z",
        "DeletedAt": null,
        "user_id": 123456,
        "plan_id": 1,
        "start_date": "2025-11-27T12:00:00Z",
        "end_date": "2025-12-27T12:00:00Z",
        "status": "active",
        "plan": {
          "ID": 1,
          "name": "Basic Plan",
          "price": 9.99,
          "duration_days": 30
        }
      }
    ]
  }
  ```

### Get Active Subscription

Retrieve the currently active subscription for the authenticated user.

- **Endpoint:** `GET /v1/subscriptions/active`
- **Authentication:** Required
- **Response (Active Found):**
  ```json
  {
    "success": true,
    "subscription": {
      "ID": 1,
      "user_id": 123456,
      "plan_id": 1,
      "start_date": "2025-11-27T12:00:00Z",
      "end_date": "2025-12-27T12:00:00Z",
      "status": "active",
      "plan": {
        "ID": 1,
        "name": "Basic Plan",
        "price": 9.99,
        "duration_days": 30
      }
    }
  }
  ```
- **Response (No Active Subscription):**
  ```json
  {
    "success": false,
    "message": "No active subscription found"
  }
  ```

### Cancel Subscription

Cancel a subscription. Status changes to `cancelled`; no refund is issued.

- **Endpoint:** `POST /v1/subscriptions/:id/cancel`
- **Authentication:** Required
- **Response:**
  ```json
  {
    "success": true,
    "message": "Subscription cancelled successfully"
  }
  ```

**⚠️ Note:** Cancelling a subscription does not provide a refund and the subscription cannot be reactivated.

---

## Rate Limiting

- **Rate Limit:** 1000 requests per 10 seconds per API key

**⚠️ Warning:** Multiple failed requests to protected endpoints will trigger a permanent IP ban. Ensure your API key is valid before making requests.

### Best Practices

- Implement exponential backoff for retries
- Cache responses when appropriate
- Monitor your API usage regularly
- Use batch operations when available

---

## Error Handling

### HTTP Status Codes

| Status Code | Meaning | Description |
|-------------|---------|-------------|
| `200` | OK | Request successful |
| `400` | Bad Request | Invalid request format or parameters |
| `401` | Unauthorized | Authentication failed or API key invalid |
| `404` | Not Found | Requested resource does not exist |
| `429` | Too Many Requests | Rate limit exceeded |
| `500` | Internal Server Error | An unexpected error occurred |

### Error Response Format

```json
{
  "success": false,
  "message": "Detailed error message",
  "error_code": "ERROR_CODE_HERE"
}
```

---

## Support

For any questions, issues, or API key requests, please contact:

- **Get API Key:** [Telegram Bot](https://t.me/G2BULKBOT)
- **Support:** [Contact Support](https://t.me/seedon)

---

© 2025 G2Bulk API. Professional gaming top-up services. Built with security and reliability in mind.

This documentation is subject to updates. Always check for the latest version to ensure compatibility with the API.
