# Pharmacy Admin Dashboard — API Endpoints Summary

## Total Endpoints: 48

---

## Authentication (1)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/login` | Admin login |

## Dashboard (3)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/dashboard/stats` | Dashboard statistics cards |
| GET | `/dashboard/recent-prescriptions` | Recent prescription listing |
| GET | `/dashboard/payment-details` | Revenue/payment breakdown |

## Products (8)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/products/stats` | Product statistics |
| GET | `/products` | List products (filtered) |
| POST | `/products` | Create product |
| GET | `/products/{id}` | Product detail |
| PUT | `/products/{id}` | Update product |
| DELETE | `/products/{id}` | Delete product |
| PATCH | `/products/{id}/stock` | Update stock |
| PATCH | `/products/{id}/discount` | Apply/remove discount |

## Categories (4)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/categories` | List categories & subcategories |
| POST | `/categories` | Create category/subcategory |
| PUT | `/categories/{id}` | Update category |
| DELETE | `/categories/{id}` | Delete category |

## Orders (9)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/orders/stats` | Order statistics |
| GET | `/orders` | List orders (filtered) |
| GET | `/orders/{id}` | Full order details |
| PATCH | `/orders/{id}/fulfillment` | Update fulfillment status |
| POST | `/orders/{id}/tracking` | Add tracking info |
| POST | `/orders/{id}/notes` | Add admin note |
| GET | `/orders/telehealth/bookings` | List telehealth bookings |
| GET | `/orders/telehealth/subscriptions` | List telehealth subscriptions |

## Prescriptions (9)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/prescriptions/stats` | Prescription statistics |
| GET | `/prescriptions` | List prescription orders |
| GET | `/prescriptions/{id}` | Prescription order detail |
| PATCH | `/prescriptions/{id}/review` | Approve/modify/reject prescription |
| POST | `/prescriptions/{id}/upload` | Upload prescription file |
| PATCH | `/prescriptions/{id}/fulfillment` | Update fulfillment status |
| PATCH | `/prescriptions/{id}/delivery-fee` | Update delivery fee |
| POST | `/prescriptions/{id}/medications` | Add medication for patient |
| GET | `/prescriptions/{id}/medications/search` | Search medications |

## Reports & Analytics (8)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/reports/overview` | Overview stats |
| GET | `/reports/sales` | Sales analytics |
| GET | `/reports/inventory` | Inventory report |
| GET | `/reports/orders` | Order analytics |
| GET | `/reports/users` | User analytics |
| GET | `/reports/deliveries` | Delivery analytics |
| GET | `/reports/wishlist` | Wishlist analytics |
| GET | `/reports/export` | Export report as PDF |

## Users (4)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users/stats` | User statistics |
| GET | `/users` | List users |
| POST | `/users` | Create user |
| PUT | `/users/{id}` | Update user |
| DELETE | `/users/{id}` | Delete user |

## M-Pesa Payments (3)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/mpesa/stats` | M-Pesa payment stats |
| GET | `/mpesa/transactions` | List transactions |
| GET | `/mpesa/transactions/{id}` | Transaction detail |

---

## Workflow Diagrams Index

| # | Diagram | File | Description |
|---|---------|------|-------------|
| 1 | Order Lifecycle | `01-order-lifecycle.mermaid` | Regular pharmacy order from placement to delivery |
| 2 | Prescription Lifecycle | `02-prescription-lifecycle.mermaid` | Prescription order from upload/consultation to completion |
| 3 | M-Pesa Payment Flow | `03-mpesa-payment-flow.mermaid` | STK push to confirmation/failure handling |
| 4 | Product Management | `04-product-management.mermaid` | Add, edit, stock update, discount, delete flows |
| 5 | Telehealth → Prescription | `05-telehealth-prescription-flow.mermaid` | Doctor consultation to prescription fulfillment |
