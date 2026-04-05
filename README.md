# 📚 Online Bookstore Platform

![Next.js](https://img.shields.io/badge/Next.js-13.6.6-blue?style=flat-square) ![Node.js](https://img.shields.io/badge/Node.js-18-green?style=flat-square) ![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

An **Online Bookstore** built with **Next.js**, providing a seamless experience for clients to purchase and read books online, sellers to manage inventory and sales, and admins to oversee the platform. The system uses **role-based access control** to manage permissions dynamically.

---

## Table of Contents

* [Project Overview](#project-overview)
* [Features](#features)

  * [User Types](#user-types)
  * [Role-Based Access Control](#role-based-access-control)
  * [Dashboards](#dashboards)
  * [Book Management](#book-management)
  * [Purchase & Payment](#purchase--payment)
* [Tech Stack](#tech-stack)
* [Setup & Installation](#setup--installation)
* [Future Enhancements](#future-enhancements)

---

## Project Overview

This project is a full-stack **Online Bookstore** designed for multiple types of users with distinct roles and capabilities. Clients can browse, purchase, and read books online, while sellers manage inventory and track sales. Admins control the platform, manage users, and create custom roles.

The platform demonstrates modern web practices including **server-side rendering (SSR), API routes, authentication, dynamic content, and role-based dashboards**.

---

## Features

### User Types

* **Client**: Browse/search books, purchase, read online, manage account, and track order history.
* **Seller**: Add/edit/delete books, track sales, manage inventory, and access analytics.
* **Admin**: Full platform control, user management, role creation, transaction monitoring, and analytics.

---

### Role-Based Access Control

* Pre-built roles: **admin, employee, seller, client**.
* Admins can **create custom roles** and assign permissions dynamically.
* Each route and dashboard is **protected based on user role**.
* Ensures **security and data isolation** between user types.

---

### Dashboards

* **Client Dashboard**: Purchased books, order history, account management.
* **Seller Dashboard**: Add/manage books, view sales and analytics, track inventory.
* **Admin Dashboard**: Manage all users, roles, orders, and platform-wide analytics.

---

### Book Management

* Upload digital (PDF, ePub) or physical books.
* Book metadata: title, author, genre, description, price, stock quantity.
* Digital books available for immediate reading after purchase.
* Clients can leave **reviews and ratings**.
* Advanced search and filtering by genre, author, price, and availability.

---

### Purchase & Payment

* Secure checkout with **Stripe / PayPal integration**.
* Supports **digital downloads** and **physical delivery**.
* Orders linked to client accounts for tracking.
* Email notifications for purchases and updates.

---

## Tech Stack

* **Frontend**: Next.js, React, CSS / Tailwind CSS, dynamic routing.
* **Backend**: Next.js API routes or Node.js server, Prisma ORM.
* **Database**: PostgreSQL / MySQL.
* **Authentication**: Role-based authentication with JWT or NextAuth.
* **Storage**: Cloud storage (AWS S3 / Cloudflare R2) for digital books and images.

---

## Setup & Installation

1. **Clone the repository**

```bash
git clone https://github.com/your-username/online-bookstore.git
cd online-bookstore
```

2. **Install dependencies**

```bash
npm install
# or
yarn install
```

3. **Configure environment variables**
   Create a `.env.local` file with your configuration:

```
DATABASE_URL=your_database_url
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your_secret_key
STRIPE_SECRET_KEY=your_stripe_key
```

4. **Run database migrations**

```bash
npx prisma migrate dev
```

5. **Start the development server**

```bash
npm run dev
# or
yarn dev
```

6. Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## Future Enhancements

* Personalized book recommendations.
* Subscription model for unlimited access.
* Multi-language support.
* Advanced analytics for sellers and admins.
* Social features: follow authors, discussion forums, book clubs.
