# 📸 PhotoPrint Hub — Full-Stack MERN Photo Printing & Order Management Platform

A modern, production-grade e-commerce application designed for high-definition custom photo printing, interactive live 3D product previews, secure payments via Razorpay, cloud photo uploads, and automated email notifications on order status transitions.

---

## 🌟 Core Features

1. **User Authentication & Authorization**:
   - Secure JWT token authentication (24-hour validity)
   - Password hashing with `bcrypt.js` (Salt factor 10)
   - Role-Based Access Control (`user` vs `admin`)
   - Profile & shipping address management

2. **Dynamic Product Catalog**:
   - Browse **Canvas Prints**, **Photo Books**, **Ceramic Mugs**, **Framed Prints**, and **Standard Lab Photo Prints**
   - Filtering by category, search keywords, price sorting, and stock status

3. **Interactive 3D Live Photo Customizer & Mockup**:
   - **Ceramic Mug**: Real-time 3D cylindrical wrap perspective with glossy highlight & body color options
   - **Handmade Frames**: Solid wood oak/black/white frames with museum mat boards
   - **Gallery Wrapped Canvas**: 3D timber depth edge simulation
   - **Hardcover Photo Books**: Layflat cover with custom title embossing
   - Real-time price calculation based on chosen size, finish, and frame materials

4. **Photo Uploads & Optimization**:
   - Multer middleware with image MIME validation & 15MB file size limit
   - Cloudinary integration with automatic format selection (`fetch_format: auto`, `quality: auto`)
   - Local static storage fallback for instant out-of-the-box local development

5. **Shopping Cart & Checkout**:
   - Client and database synchronized cart state
   - Per-item custom photo references, quantity steppers, and customization badges
   - Coupon codes (`PHOTO10`, `FREESHIP`)

6. **Razorpay Payment Gateway**:
   - Server-side Razorpay order generation
   - Client-side checkout modal with HMAC SHA256 signature verification
   - Built-in test simulator mode if Razorpay API keys are not provided

7. **Automated Nodemailer Order Status Emails**:
   - Dispatches branded HTML emails for every order lifecycle update:
     - `Pending` ➔ `Confirmed` ➔ `Processing` ➔ `Dispatched` ➔ `Delivered`
   - Real-time customer order history with visual stepper timeline and printable receipt

8. **Comprehensive Admin Dashboard**:
   - Real-time KPI metrics: Total Revenue, Total Orders, In-Production Orders, Active Users
   - Order management: Update status with notes & trigger automated customer emails
   - Product Catalog CRUD & inventory manager
   - Customer and staff user directory with role promotion/demotion

---

## 🏗️ Architecture & Tech Stack

```
Three-Tier Architecture:
┌────────────────────────────────────────────────────────┐
│                   Presentation Layer                   │
│         React.js + Vite + Tailwind CSS + Lucide        │
└───────────────────────────┬────────────────────────────┘
                            │ (REST API & JWT)
┌───────────────────────────▼────────────────────────────┐
│                  Business Logic Layer                  │
│       Node.js + Express.js + Multer + Nodemailer       │
└──────────────┬───────────────────────────┬─────────────┘
               │                           │
┌──────────────▼──────────────┐ ┌──────────▼─────────────┐
│          Data Layer         │ │     External Services  │
│   MongoDB + Mongoose ODM    │ │  Cloudinary + Razorpay │
└─────────────────────────────┘ └────────────────────────┘
```

- **Frontend**: React 18, Vite, Tailwind CSS, Lucide Icons, Axios, React Router v6
- **Backend**: Node.js, Express.js, Mongoose ODM, JWT, bcryptjs, Multer, Nodemailer
- **Database**: MongoDB (Local or MongoDB Atlas)
- **External Services**: Cloudinary (Image storage), Razorpay (Payments)

---

## 📁 Project Structure

```
photo-print-hub/
├── server/
│   ├── src/
│   │   ├── config/             # DB, Cloudinary, Razorpay, Nodemailer configs
│   │   ├── controllers/        # auth, product, order, cart, payment, admin, upload controllers
│   │   ├── middleware/         # auth (protect, admin), upload (multer), errorHandler
│   │   ├── models/             # User, Product, Order, Cart, Payment
│   │   ├── routes/             # Express API routes
│   │   ├── services/           # emailService (Nodemailer HTML notifications)
│   │   ├── seed/               # seed.js (pre-populate sample products & accounts)
│   │   └── server.js           # Server entrypoint
│   ├── .env.example
│   ├── package.json
│
├── client/
│   ├── src/
│   │   ├── components/         # Navbar, Footer, LiveCustomizerPreview, ProductCard, StatusBadge, etc.
│   │   ├── context/            # AuthContext, CartContext
│   │   ├── pages/              # Home, Products, ProductDetail, Cart, Checkout, OrderSuccess, OrderHistory, OrderDetail, Admin pages
│   │   ├── services/           # api.js (Axios with JWT interceptor)
│   │   ├── App.jsx             # Main router
│   │   └── main.jsx
│   ├── index.html
│   ├── tailwind.config.js
│   ├── vite.config.js
│   └── package.json
│
├── package.json                # Root package for running client & server
└── README.md
```

---

## 🔑 Demo Accounts

For immediate testing, run the seed script to generate these pre-configured accounts:

| Role | Email | Password | Permissions |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin@photoprint.com` | `admin123` | Full admin dashboard, status changes, product CRUD, user roles |
| **Customer** | `customer@photoprint.com` | `user123` | Photo customizer, cart, checkout, order history & tracking |

---

## 🚀 Quick Start Guide

### 1. Prerequisites
- [Node.js](https://nodejs.org/) (v18 or higher)
- [MongoDB](https://www.mongodb.com/) running locally on port 27017 or a MongoDB Atlas URI

### 2. Configure Environment Variables
Copy `server/.env.example` to `server/.env`:
```bash
cd server
cp .env.example .env
```

*(Optional)* Fill in your Cloudinary, Razorpay, or Gmail SMTP credentials in `server/.env`. If left empty, resilient development simulation fallbacks are automatically active!

### 3. Seed Database with Realistic Products
```bash
cd server
npm run seed
```

### 4. Run the Backend & Frontend

**Terminal 1 — Backend:**
```bash
cd server
npm run dev
# Server running on http://localhost:5000
```

**Terminal 2 — Frontend:**
```bash
cd client
npm run dev
# Client running on http://localhost:5173
```

---

## 📡 Key REST API Endpoints

### Auth
- `POST /api/auth/register` — Register a new account
- `POST /api/auth/login` — Login & receive 24h JWT token
- `GET /api/auth/profile` — Get current user profile *(Private)*
- `PUT /api/auth/profile` — Update address & personal details *(Private)*

### Products
- `GET /api/products` — Get catalog with category & price filters
- `GET /api/products/:id` — Get product customizer specs

### Photo Uploads
- `POST /api/upload` — Upload photo (Multer / Cloudinary)

### Cart
- `GET /api/cart` — Get user's cart *(Private)*
- `POST /api/cart` — Add customized photo item *(Private)*
- `PUT /api/cart/:itemId` — Update quantity *(Private)*
- `DELETE /api/cart/:itemId` — Remove item *(Private)*

### Orders & Payments
- `POST /api/orders` — Place order *(Private)*
- `GET /api/orders/history` — Get customer order history *(Private)*
- `GET /api/orders/:id` — View tracking timeline & invoice *(Private)*
- `POST /api/payment/create-order` — Create Razorpay order *(Private)*
- `POST /api/payment/verify` — Verify Razorpay signature *(Private)*

### Admin
- `GET /api/admin/stats` — Dashboard metrics *(Admin)*
- `GET /api/admin/orders` — Manage all orders *(Admin)*
- `PUT /api/admin/orders/:id/status` — Update order status & trigger email *(Admin)*
- `POST /api/admin/products` — Add product *(Admin)*
- `PUT /api/admin/products/:id` — Edit product *(Admin)*
- `DELETE /api/admin/products/:id` — Remove product *(Admin)*
- `GET /api/admin/users` — List registered users *(Admin)*
- `PUT /api/admin/users/:id/role` — Promote/demote role *(Admin)*

---

## 📄 License
MIT License. Built with ❤️ for photographers and memory keepers.
