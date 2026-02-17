# Pharmacy Admin Dashboard — API Documentation

## Table of Contents

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [API Endpoints](#api-endpoints)
   - [Dashboard](#1-dashboard)
   - [Products](#2-products)
   - [Categories](#3-categories)
   - [Orders](#4-orders)
   - [Prescriptions](#5-prescription-order-management)
   - [Reports & Analytics](#6-reports--analytics)
   - [Users](#7-users)
   - [M-Pesa Payments](#8-mpesa-payments)

---

## Overview

**Base URL:** `https://api.example.com/api/v1`

All API responses follow a standard envelope:

```json
{
  "success": true,
  "message": "Operation successful",
  "data": { },
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 150,
    "total_pages": 8
  }
}
```

Error responses:

```json
{
  "success": false,
  "message": "Validation failed",
  "errors": {
    "field_name": ["Error message"]
  }
}
```

---

## Authentication

All endpoints require a Bearer token in the `Authorization` header.

```
Authorization: Bearer <jwt_token>
```

### POST `/auth/login`

**Request:**
```json
{
  "email": "admin@pharmacy.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "user": {
      "id": 1,
      "name": "Admin User",
      "email": "admin@pharmacy.com",
      "role": "admin"
    }
  }
}
```

---

## API Endpoints

---

### 1. Dashboard

#### GET `/dashboard/stats`

Returns all dashboard stat cards.

**Response:**
```json
{
  "success": true,
  "data": {
    "total_revenue": 1250000.00,
    "todays_revenue": 45000.00,
    "total_orders": 3420,
    "pending_orders": 18,
    "total_prescriptions": 890,
    "pending_prescription_reviews": 12,
    "total_users": 5600,
    "total_products": 1240
  }
}
```

#### GET `/dashboard/recent-prescriptions`

Returns recent prescription orders for the dashboard listing.

**Query Params:** `?page=1&per_page=10`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "RX-20260215-000045",
      "customer": {
        "id": 102,
        "name": "Jane Doe",
        "phone": "+254712345678"
      },
      "status": "under_review",
      "payment": "pending",
      "amount": 2500.00,
      "created_at": "2026-02-15T10:30:00Z"
    }
  ],
  "meta": { "page": 1, "per_page": 10, "total": 45, "total_pages": 5 }
}
```

#### GET `/dashboard/payment-details`

Returns detailed payment/revenue breakdown (linked from Total Revenue card).

**Response:**
```json
{
  "success": true,
  "data": {
    "total_revenue": 1250000.00,
    "revenue_by_method": {
      "mpesa": 980000.00,
      "card": 150000.00,
      "cash": 120000.00
    },
    "revenue_by_period": {
      "today": 45000.00,
      "this_week": 210000.00,
      "this_month": 680000.00
    },
    "recent_payments": [
      {
        "id": "TXN-001",
        "order_id": "ORD-20260215-001",
        "customer": "Jane Doe",
        "amount": 2500.00,
        "method": "mpesa",
        "status": "completed",
        "date": "2026-02-15T10:30:00Z"
      }
    ]
  }
}
```

---

### 2. Products

#### GET `/products/stats`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_products": 1240,
    "active_products": 1100,
    "inactive_products": 140,
    "low_stock": 45,
    "out_of_stock": 12,
    "with_offers": 38,
    "prescription_products": 320,
    "featured_products": 25,
    "popular_products": 50
  }
}
```

#### GET `/products`

List products with filtering.

**Query Params:**
| Param | Type | Description |
|-------|------|-------------|
| `category_id` | integer | Filter by category |
| `stock_status` | string | `in_stock`, `low_stock`, `out_of_stock` |
| `status` | string | `active`, `inactive` |
| `search` | string | Search by name, SKU, brand |
| `page` | integer | Page number |
| `per_page` | integer | Items per page |
| `sort_by` | string | `name`, `price`, `stock`, `created_at` |
| `sort_order` | string | `asc`, `desc` |

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Amoxicillin 500mg",
      "sku": "AMX-500-30",
      "image_url": "https://cdn.example.com/products/amx500.jpg",
      "category": {
        "id": 5,
        "name": "Antibiotics"
      },
      "subcategory": {
        "id": 12,
        "name": "Penicillins"
      },
      "price": 850.00,
      "stock": {
        "quantity": 120,
        "status": "in_stock"
      },
      "status": "active",
      "requires_prescription": true,
      "featured": false
    }
  ],
  "meta": { "page": 1, "per_page": 20, "total": 1240, "total_pages": 62 }
}
```

#### GET `/products/{id}`

**Response:**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Amoxicillin 500mg",
    "sku": "AMX-500-30",
    "category_id": 5,
    "subcategory_id": 12,
    "brand": "GSK",
    "generic_name": "Amoxicillin",
    "manufacturer": "GlaxoSmithKline",
    "short_description": "Broad-spectrum antibiotic",
    "full_description": "Amoxicillin is used to treat a wide variety of bacterial infections...",
    "dosage_form": "capsule",
    "strength": "500mg",
    "pack_size": "30 capsules",
    "selling_price": 850.00,
    "compare_at_price": 1000.00,
    "cost_price": 500.00,
    "stock_quantity": 120,
    "low_stock_threshold": 20,
    "image_url": "https://cdn.example.com/products/amx500.jpg",
    "requires_prescription": true,
    "is_active": true,
    "is_featured": false,
    "discount": {
      "type": "percentage",
      "value": 15,
      "discounted_price": 850.00
    },
    "created_at": "2026-01-10T08:00:00Z",
    "updated_at": "2026-02-14T12:00:00Z"
  }
}
```

#### POST `/products`

Create a new product.

**Request (multipart/form-data):**
```json
{
  "name": "Amoxicillin 500mg",
  "sku": "AMX-500-30",
  "category_id": 5,
  "subcategory_id": 12,
  "brand": "GSK",
  "generic_name": "Amoxicillin",
  "manufacturer": "GlaxoSmithKline",
  "short_description": "Broad-spectrum antibiotic",
  "full_description": "Full product description...",
  "dosage_form": "capsule",
  "strength": "500mg",
  "pack_size": "30 capsules",
  "selling_price": 850.00,
  "compare_at_price": 1000.00,
  "cost_price": 500.00,
  "stock_quantity": 120,
  "low_stock_threshold": 20,
  "image": "<binary file>",
  "requires_prescription": true,
  "is_active": true,
  "is_featured": false
}
```

**Required fields:** `name`, `sku`, `category_id`, `selling_price`, `stock_quantity`

**Response:** `201 Created`
```json
{
  "success": true,
  "message": "Product created successfully",
  "data": { "id": 1241, "...": "...full product object" }
}
```

#### PUT `/products/{id}`

Update product. Same body as POST (all fields optional).

**Response:** `200 OK`

#### DELETE `/products/{id}`

**Response:**
```json
{
  "success": true,
  "message": "Product deleted successfully"
}
```

#### PATCH `/products/{id}/stock`

Update stock for a product.

**Request:**
```json
{
  "adjustment_type": "add",
  "quantity": 50,
  "reason": "New shipment received"
}
```

| `adjustment_type` | Description |
|---|---|
| `set` | Set to exact quantity |
| `add` | Add to current stock |
| `subtract` | Subtract from current stock |

**Response:**
```json
{
  "success": true,
  "message": "Stock updated successfully",
  "data": {
    "product_id": 1,
    "previous_stock": 120,
    "new_stock": 170,
    "adjustment_type": "add",
    "quantity": 50,
    "reason": "New shipment received"
  }
}
```

#### PATCH `/products/{id}/discount`

Apply or remove a discount/offer.

**Request (apply):**
```json
{
  "action": "apply",
  "discount_type": "percentage",
  "discount_value": 10
}
```

**Request (remove):**
```json
{
  "action": "remove"
}
```

| `discount_type` | Example | Effect |
|---|---|---|
| `percentage` | `10` | 10% off selling price |
| `fixed` | `100` | KES 100 off selling price |

**Response:**
```json
{
  "success": true,
  "message": "Discount applied successfully",
  "data": {
    "product_id": 1,
    "original_price": 1000.00,
    "discount_type": "percentage",
    "discount_value": 10,
    "discounted_price": 900.00
  }
}
```

---

### 3. Categories

#### GET `/categories`

**Query Params:** `?type=category|subcategory&status=active|inactive&search=name`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Antibiotics",
      "image_url": "https://cdn.example.com/categories/antibiotics.png",
      "icon": "pill",
      "parent_id": null,
      "product_count": 85,
      "order_count": 320,
      "status": "active",
      "subcategories": [
        {
          "id": 12,
          "name": "Penicillins",
          "parent_id": 1,
          "product_count": 30,
          "order_count": 120,
          "status": "active"
        }
      ]
    }
  ]
}
```

#### POST `/categories`

**Request (multipart/form-data):**
```json
{
  "name": "Antibiotics",
  "parent_id": null,
  "status": "active",
  "image": "<binary file>"
}
```

Set `parent_id` to a category ID to create a subcategory.

**Response:** `201 Created`
```json
{
  "success": true,
  "message": "Category created successfully",
  "data": { "id": 15, "name": "Antibiotics", "..." : "..." }
}
```

#### PUT `/categories/{id}`

**Request:**
```json
{
  "name": "Updated Name",
  "status": "inactive",
  "image": "<binary file optional>"
}
```

#### DELETE `/categories/{id}`

**Response:**
```json
{
  "success": true,
  "message": "Category deleted successfully"
}
```

> **Note:** Deletion fails if category has associated products. Response will return `400` with appropriate message.

---

### 4. Orders

#### GET `/orders/stats`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_orders": 3420,
    "pending": 18,
    "processing": 25,
    "dispatched": 10,
    "completed": 3300,
    "unpaid": 8,
    "todays_revenue": 45000.00
  }
}
```

#### GET `/orders`

**Query Params:**
| Param | Type | Description |
|-------|------|-------------|
| `type` | string | `pharmacy`, `telehealth` |
| `status` | string | `pending`, `processing`, `dispatched`, `completed`, `cancelled` |
| `payment_status` | string | `paid`, `unpaid`, `partial` |
| `date_from` | date | Start date (YYYY-MM-DD) |
| `date_to` | date | End date (YYYY-MM-DD) |
| `search` | string | Search by order ID, customer name |
| `page` | integer | Page number |
| `per_page` | integer | Items per page |

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "ORD-20260215-001",
      "customer": {
        "id": 102,
        "name": "Jane Doe"
      },
      "items_count": 3,
      "total": 4500.00,
      "payment_status": "paid",
      "status": "processing",
      "date": "2026-02-15T10:30:00Z",
      "type": "pharmacy"
    }
  ],
  "meta": { "page": 1, "per_page": 20, "total": 3420, "total_pages": 171 }
}
```

#### GET `/orders/{id}`

Full order details.

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "ORD-20260215-001",
    "type": "pharmacy",
    "customer": {
      "id": 102,
      "name": "Jane Doe",
      "phone": "+254712345678",
      "email": "jane@example.com",
      "gender": "female",
      "dob": "1990-05-15",
      "insurance": {
        "provider": "Jubilee Insurance",
        "policy_number": "JUB-2026-12345",
        "member_number": "M-9876"
      }
    },
    "delivery": {
      "zone": "Nairobi CBD",
      "address": "123 Kenyatta Ave, Nairobi"
    },
    "items": [
      {
        "id": 1,
        "product_id": 45,
        "name": "Amoxicillin 500mg",
        "sku": "AMX-500-30",
        "quantity": 2,
        "unit_price": 850.00,
        "total": 1700.00,
        "image_url": "https://cdn.example.com/products/amx500.jpg"
      }
    ],
    "order_summary": {
      "subtotal": 4200.00,
      "delivery_fee": 300.00,
      "discount": 0.00,
      "total": 4500.00
    },
    "payment": {
      "method": "mpesa",
      "status": "paid",
      "transaction_id": "TXN-MPESA-001",
      "paid_at": "2026-02-15T10:35:00Z"
    },
    "nextech_sync": {
      "status": "posted",
      "posted_at": "2026-02-15T10:36:00Z",
      "items_posted": true
    },
    "fulfillment": {
      "status": "processing",
      "workflow": [
        { "step": "pending", "completed": true, "at": "2026-02-15T10:30:00Z" },
        { "step": "received", "completed": true, "at": "2026-02-15T10:40:00Z" },
        { "step": "preparing", "completed": false, "at": null },
        { "step": "ready", "completed": false, "at": null },
        { "step": "dispatched", "completed": false, "at": null },
        { "step": "delivered", "completed": false, "at": null }
      ]
    },
    "tracking": {
      "tracking_number": null,
      "carrier": null,
      "url": null
    },
    "admin_notes": [
      {
        "id": 1,
        "note": "Customer requested morning delivery",
        "author": "Admin User",
        "created_at": "2026-02-15T11:00:00Z"
      }
    ],
    "created_at": "2026-02-15T10:30:00Z",
    "updated_at": "2026-02-15T10:40:00Z"
  }
}
```

#### PATCH `/orders/{id}/fulfillment`

Update fulfillment status.

**Request:**
```json
{
  "status": "preparing"
}
```

Allowed values: `pending`, `received`, `preparing`, `ready`, `dispatched`, `delivered`

**Response:**
```json
{
  "success": true,
  "message": "Fulfillment status updated to preparing",
  "data": {
    "order_id": "ORD-20260215-001",
    "status": "preparing",
    "updated_at": "2026-02-15T11:30:00Z"
  }
}
```

#### POST `/orders/{id}/tracking`

Add tracking information.

**Request:**
```json
{
  "tracking_number": "TRK-123456",
  "carrier": "Sendy",
  "url": "https://tracking.sendy.co.ke/TRK-123456"
}
```

#### POST `/orders/{id}/notes`

Add admin note.

**Request:**
```json
{
  "note": "Customer called to confirm delivery window"
}
```

#### GET `/orders/telehealth/bookings`

**Query Params:** `?page=1&per_page=10&search=`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "TH-001",
      "customer": { "id": 102, "name": "Jane Doe" },
      "items": ["General Consultation"],
      "total": 1500.00,
      "payment_status": "paid",
      "date": "2026-02-15T14:00:00Z"
    }
  ]
}
```

#### GET `/orders/telehealth/subscriptions`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "SUB-001",
      "customer": { "id": 102, "name": "Jane Doe" },
      "service": "Mental Health",
      "package": "Monthly",
      "price": 5000.00,
      "status": "active",
      "date": "2026-01-15T00:00:00Z"
    }
  ]
}
```

---

### 5. Prescription Order Management

#### GET `/prescriptions/stats`

**Response:**
```json
{
  "success": true,
  "data": {
    "total": 890,
    "under_review": 12,
    "payment_pending": 8,
    "paid": 850,
    "dispatched": 10,
    "delivered": 810
  }
}
```

#### GET `/prescriptions`

**Query Params:**
| Param | Type | Description |
|-------|------|-------------|
| `payment_status` | string | `pending`, `paid`, `failed` |
| `fulfillment_status` | string | `under_review`, `approved`, `rejected`, `uploaded`, `consultation_done`, `paid`, `preparing`, `dispatched`, `delivered`, `completed` |
| `date_from` | date | Start date |
| `date_to` | date | End date |
| `search` | string | Search by order ID, customer name |
| `page` | integer | Page number |
| `per_page` | integer | Items per page |

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "RX-20260215-000045",
      "customer": { "id": 102, "name": "Jane Doe" },
      "status": "under_review",
      "payment_status": "pending",
      "items_count": 2,
      "amount": 2500.00,
      "created_at": "2026-02-15T10:30:00Z"
    }
  ],
  "meta": { "page": 1, "per_page": 20, "total": 890, "total_pages": 45 }
}
```

#### GET `/prescriptions/{id}`

Full prescription order detail.

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "RX-20260215-000045",
    "customer": {
      "id": 102,
      "name": "Jane Doe",
      "phone": "+254712345678",
      "email": "jane@example.com",
      "gender": "female",
      "dob": "1990-05-15",
      "insurance": {
        "provider": "Jubilee Insurance",
        "policy_number": "JUB-2026-12345"
      }
    },
    "items": [
      {
        "id": 1,
        "product_id": 45,
        "name": "Amoxicillin 500mg",
        "quantity": 2,
        "unit_price": 850.00,
        "total": 1700.00,
        "dosage": "500mg",
        "frequency": "3 times daily",
        "duration": "7 days",
        "instructions": "Take after meals"
      }
    ],
    "order_summary": {
      "order_number": "RX-20260215-000045",
      "date": "2026-02-15T10:30:00Z",
      "status": "under_review",
      "subtotal": 2200.00,
      "delivery_fee": 300.00,
      "total": 2500.00
    },
    "nextech_sync": {
      "status": "pending",
      "posted_at": null,
      "items_posted": false
    },
    "prescription_review": {
      "status": "under_review",
      "reviewed_by": null,
      "reviewed_at": null,
      "notes": null
    },
    "prescription_file": {
      "uploaded": true,
      "file_url": "https://cdn.example.com/prescriptions/rx-045.pdf",
      "uploaded_by": "customer",
      "uploaded_at": "2026-02-15T10:28:00Z"
    },
    "fulfillment": {
      "enabled": false,
      "status": "under_review",
      "workflow": [
        { "step": "uploaded", "completed": true, "at": "2026-02-15T10:28:00Z" },
        { "step": "doctor_consultation_done", "completed": false, "at": null },
        { "step": "paid", "completed": false, "at": null },
        { "step": "preparing", "completed": false, "at": null },
        { "step": "dispatched", "completed": false, "at": null },
        { "step": "delivered", "completed": false, "at": null },
        { "step": "completed", "completed": false, "at": null }
      ]
    },
    "medications": [
      {
        "id": 1,
        "product_id": 45,
        "name": "Amoxicillin 500mg",
        "quantity": 2,
        "dosage": "500mg",
        "frequency": "3 times daily",
        "duration": "7 days",
        "instructions": "Take after meals",
        "added_by": "pharmacist"
      }
    ],
    "created_at": "2026-02-15T10:30:00Z",
    "updated_at": "2026-02-15T10:30:00Z"
  }
}
```

#### PATCH `/prescriptions/{id}/review`

Approve, request modification, or reject a prescription.

**Request:**
```json
{
  "action": "approve",
  "notes": "Prescription verified and approved"
}
```

| `action` | Description |
|---|---|
| `approve` | Approve & notify customer |
| `request_modification` | Request modification & notify |
| `reject` | Reject & notify |

**Response:**
```json
{
  "success": true,
  "message": "Prescription approved. Customer notified.",
  "data": {
    "id": "RX-20260215-000045",
    "review_status": "approved",
    "fulfillment_enabled": true,
    "notified_at": "2026-02-15T11:00:00Z"
  }
}
```

#### POST `/prescriptions/{id}/upload`

Upload a prescription file (by pharmacist on behalf of patient).

**Request (multipart/form-data):**
```
file: <binary PDF/image>
```

**Response:**
```json
{
  "success": true,
  "message": "Prescription uploaded successfully",
  "data": {
    "file_url": "https://cdn.example.com/prescriptions/rx-045.pdf",
    "uploaded_by": "pharmacist",
    "uploaded_at": "2026-02-15T11:05:00Z"
  }
}
```

#### PATCH `/prescriptions/{id}/fulfillment`

Update fulfillment status (only enabled after prescription approval).

**Request:**
```json
{
  "status": "preparing"
}
```

Allowed: `uploaded`, `doctor_consultation_done`, `paid`, `preparing`, `dispatched`, `delivered`, `completed`

#### PATCH `/prescriptions/{id}/delivery-fee`

Update delivery fee for the prescription order.

**Request:**
```json
{
  "delivery_fee": 350.00
}
```

#### POST `/prescriptions/{id}/medications`

Add medication on behalf of the patient.

**Request:**
```json
{
  "product_id": 45,
  "quantity": 2,
  "dosage": "500mg",
  "frequency": "3 times daily",
  "duration": "7 days",
  "instructions": "Take after meals"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Medication added successfully",
  "data": {
    "id": 3,
    "product_id": 45,
    "name": "Amoxicillin 500mg",
    "quantity": 2,
    "dosage": "500mg",
    "frequency": "3 times daily",
    "duration": "7 days",
    "instructions": "Take after meals",
    "added_by": "pharmacist"
  }
}
```

#### GET `/prescriptions/{id}/medications/search`

Search products for adding medication.

**Query Params:** `?q=amoxicillin`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 45,
      "name": "Amoxicillin 500mg",
      "sku": "AMX-500-30",
      "price": 850.00,
      "stock": 120,
      "requires_prescription": true
    }
  ]
}
```

---

### 6. Reports & Analytics

#### GET `/reports/overview`

**Query Params:**
| Param | Type | Description |
|-------|------|-------------|
| `period` | string | `today`, `this_week`, `this_month`, `this_year`, `custom` |
| `start_date` | date | Required if period=custom |
| `end_date` | date | Required if period=custom |

**Response:**
```json
{
  "success": true,
  "data": {
    "todays_sales": { "amount": 45000.00, "orders": 32 },
    "this_week": { "amount": 210000.00, "orders": 180 },
    "this_month": { "amount": 680000.00, "orders": 520 },
    "this_year": { "amount": 1250000.00, "orders": 3420 }
  }
}
```

#### GET `/reports/sales`

**Query Params:** Same period filters.

**Response:**
```json
{
  "success": true,
  "data": {
    "revenue_by_category": [
      { "category": "Antibiotics", "revenue": 320000.00 },
      { "category": "Pain Relief", "revenue": 180000.00 }
    ],
    "sales_trend_30_days": [
      { "date": "2026-01-17", "revenue": 22000.00 },
      { "date": "2026-01-18", "revenue": 25000.00 }
    ],
    "top_products": [
      {
        "name": "Amoxicillin 500mg",
        "orders": 245,
        "sold_units": 490,
        "revenue": 416500.00
      }
    ]
  }
}
```

#### GET `/reports/inventory`

**Response:**
```json
{
  "success": true,
  "data": {
    "in_stock": 1180,
    "out_of_stock": 12,
    "low_stock": 45,
    "fast_moving": [
      { "product_id": 45, "name": "Amoxicillin 500mg", "sold_last_30_days": 490 }
    ],
    "low_stock_alerts": [
      { "product_id": 78, "name": "Paracetamol 500mg", "stock": 8, "threshold": 20 }
    ],
    "out_of_stock_alerts": [
      { "product_id": 99, "name": "Cetirizine 10mg", "last_in_stock": "2026-02-10" }
    ]
  }
}
```

#### GET `/reports/orders`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_orders": 3420,
    "pending": 18,
    "completed": 3300,
    "cancelled": 102,
    "orders_by_type": {
      "prescription_orders": 890,
      "regular_orders": 2400,
      "wishlist_conversions": 130
    },
    "turnaround_performance": {
      "average_turnaround_minutes": 145,
      "on_time_delivery_percentage": 92.5,
      "fastest_order_minutes": 22,
      "slowest_order_minutes": 480
    }
  }
}
```

#### GET `/reports/users`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_users": 5600,
    "active_users": 3200,
    "new_users_30_days": 320
  }
}
```

#### GET `/reports/deliveries`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_deliveries": 3310,
    "completed": 3200,
    "in_transit": 10,
    "average_delivery_time_minutes": 90
  }
}
```

#### GET `/reports/wishlist`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_wishlists": 4500,
    "unique_users": 2100,
    "converted_orders": 130,
    "conversion_rate": 2.89,
    "most_wishlisted": [
      {
        "product_name": "Vitamin D3 1000IU",
        "price": 1200.00,
        "times_wishlisted": 340,
        "conversions": 45,
        "conversion_rate": 13.24
      }
    ]
  }
}
```

#### GET `/reports/export`

Export report as PDF.

**Query Params:** `?type=sales|inventory|orders|users|deliveries|wishlist&period=this_month&start_date=&end_date=`

**Response:** Binary PDF file with `Content-Type: application/pdf`

---

### 7. Users

#### GET `/users/stats`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_users": 5600,
    "admin_staff": 12
  }
}
```

#### GET `/users`

**Query Params:** `?role=admin|staff|customer&search=&page=1&per_page=20`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Admin User",
      "email": "admin@pharmacy.com",
      "phone": "+254700000000",
      "role": "admin",
      "status": "active",
      "created_at": "2025-01-01T00:00:00Z"
    }
  ]
}
```

#### POST `/users`

**Request:**
```json
{
  "name": "New Staff",
  "email": "staff@pharmacy.com",
  "phone": "+254711111111",
  "role": "staff",
  "password": "securePassword123"
}
```

#### PUT `/users/{id}`

Update user details. Same body as POST (all fields optional).

#### DELETE `/users/{id}`

---

### 8. MPESA Payments

#### GET `/mpesa/stats`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_revenue": 980000.00,
    "todays_transactions": 45,
    "successful": 42,
    "pending": 2,
    "failed": 1
  }
}
```

#### GET `/mpesa/transactions`

**Query Params:**
| Param | Type | Description |
|-------|------|-------------|
| `status` | string | `successful`, `pending`, `failed` |
| `date_from` | date | Start date |
| `date_to` | date | End date |
| `search` | string | Transaction ID, order number, name, phone |
| `page` | integer | Page number |
| `per_page` | integer | Items per page |

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "transaction_id": "TXN-MPESA-RG7H2K",
      "order_id": "ORD-20260215-001",
      "customer": {
        "name": "Jane Doe",
        "phone": "+254712345678"
      },
      "amount": 4500.00,
      "status": "successful",
      "date": "2026-02-15T10:35:00Z"
    }
  ],
  "meta": { "page": 1, "per_page": 20, "total": 980, "total_pages": 49 }
}
```

#### GET `/mpesa/transactions/{transaction_id}`

**Response:**
```json
{
  "success": true,
  "data": {
    "transaction_id": "TXN-MPESA-RG7H2K",
    "mpesa_receipt": "RG7H2KABC",
    "order_id": "ORD-20260215-001",
    "customer": {
      "name": "Jane Doe",
      "phone": "+254712345678"
    },
    "amount": 4500.00,
    "status": "successful",
    "initiated_at": "2026-02-15T10:34:00Z",
    "completed_at": "2026-02-15T10:35:00Z",
    "callback_data": {}
  }
}
```

---

## Enum Reference

### Order Status
`pending` | `received` | `preparing` | `ready` | `dispatched` | `delivered` | `cancelled`

### Prescription Fulfillment Status
`uploaded` | `doctor_consultation_done` | `paid` | `preparing` | `dispatched` | `delivered` | `completed`

### Prescription Review Status
`under_review` | `approved` | `modification_requested` | `rejected`

### Payment Status
`pending` | `paid` | `failed` | `refunded`

### Product Stock Status
`in_stock` | `low_stock` | `out_of_stock`

### Dosage Forms
`tablet` | `capsule` | `syrup` | `injection` | `cream` | `ointment` | `drops` | `inhaler` | `suppository` | `powder` | `suspension` | `gel` | `patch` | `spray`

### User Roles
`admin` | `staff` | `pharmacist` | `doctor` | `customer`
