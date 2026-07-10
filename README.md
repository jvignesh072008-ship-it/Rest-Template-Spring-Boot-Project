# Rest-Template-Spring-Boot-Project
# 🍔 Swiggy Clone – Microservices Project

A **Swiggy-inspired Food Delivery Platform** built using **Spring Boot Microservices**. The project currently consists of **two** independent services that communicate with each other using **REST APIs** via **RestTemplate**.

Each microservice has its own independent H2 database, following the **Database per Service** pattern.

---
 
# 📌 Architecture

```text
                    REST (RestTemplate)
+--------------------------------------------------------------+

      +-------------------------+
      |      Swiggy Service     |
      |       Port : 8082       |
      |-------------------------|
      | Users                   |
      | Menu Items              |
      | Orders                  |
      | Payments                |
      +-----------+-------------+
                  ^
                  |
                  |
      +-----------+-------------+
      | Restaurant Management   |
      |      Port : 8083        |
      |-------------------------|
      | Restaurants              |
      | Calls Swiggy Service     |
      | APIs via RestTemplate    |
      +-------------------------+
```

> ℹ️ A **Delivery Management Service** is referenced in earlier notes/diagrams for this project, but no such module is present in the current codebase. This document reflects only the two services that actually exist: **Swiggy Service** and **Restaurant Management Service**.

---

# 📖 Project Overview

The project is currently made up of two independent Spring Boot applications.

## 1. Swiggy Service (Port: 8082)

Responsible for:

- User Management
- Menu Item Management
- Order Management
- Payment Management

---

## 2. Restaurant Management Service (Port: 8083)

Responsible for:

- Restaurant Management
- Cuisine Filtering
- Active Restaurant Status
- Fetching Menu Items from Swiggy Service using RestTemplate

---

# 🚀 Tech Stack

- Java 26
- Spring Boot 4.1.0
- Spring Web
- Spring Data JPA
- Spring Validation
- H2 Database (both services)
- MySQL Connector/J (present as a dependency in the Swiggy service, currently unused — the service is wired to H2 via `application.properties`)
- Lombok
- Maven
- RestTemplate

---

# 📂 Microservices

| Service | Port | Description |
|----------|------|-------------|
| Swiggy | 8082 | Users, Items, Orders, Payments |
| Restaurant Management | 8083 | Restaurants & External Item APIs |

---

# 🗄 Database

Each service owns its own H2 in-memory database.

| Service | Database |
|----------|----------|
| Swiggy | `swiggydb` (H2) |
| Restaurant Management | `restaurantdb` (H2) |

No database is shared between services.

---

# ▶ Running the Project

Open two terminals.

## Terminal 1

```bash
cd swiggy
./mvnw spring-boot:run
```

Runs on

```
http://localhost:8082
```

---

## Terminal 2

```bash
cd restaurant-management-service
./mvnw spring-boot:run
```

Runs on

```
http://localhost:8083
```

> The Restaurant Management Service calls the Swiggy Service at the URL configured in `swiggy.clone.base-url` (default `http://localhost:8082`). Start the Swiggy Service first.

---

# 💾 H2 Console

| Service | URL |
|----------|-----|
| Swiggy | http://localhost:8082/h2-console |
| Restaurant Management | http://localhost:8083/h2-console |

---

# 📌 API Reference

---

# 🍽 Swiggy Service

Base URL

```
http://localhost:8082
```

## Users

| Method | Endpoint |
|--------|----------|
| POST | /api/users |
| GET | /api/users |
| GET | /api/users/{id} |
| PUT | /api/users/{id} |
| DELETE | /api/users/{id} |

---

## Items

| Method | Endpoint |
|--------|----------|
| POST | /api/items |
| GET | /api/items |
| GET | /api/items/{id} |
| PUT | /api/items/{id} |
| PATCH | /api/items/{id}/toggle-availability |
| DELETE | /api/items/{id} |

---

## Orders

| Method | Endpoint |
|--------|----------|
| POST | /api/orders |
| GET | /api/orders (optional `userId` query param to filter by user) |
| GET | /api/orders/{id} |
| PATCH | /api/orders/{id}/status?status={status} |
| PATCH | /api/orders/{id}/cancel |

---

## Payments

| Method | Endpoint |
|--------|----------|
| POST | /api/payments |
| GET | /api/payments (optional `orderId` query param to filter by order) |
| GET | /api/payments/{id} |
| PATCH | /api/payments/{id}/status?status={status} |

---

# 🍴 Restaurant Management Service

Base URL

```
http://localhost:8083
```

## Restaurants

| Method | Endpoint |
|--------|----------|
| POST | /api/restaurants |
| GET | /api/restaurants |
| GET | /api/restaurants/{id} |
| GET | /api/restaurants/{id}/active |
| GET | /api/restaurants/cuisine/{cuisineType} |
| PUT | /api/restaurants/{id} |
| DELETE | /api/restaurants/{id} |

---

## External APIs

| Method | Endpoint |
|--------|----------|
| GET | /api/external/items |
| GET | /api/external/items/{itemId} |
| GET | /api/external/items/available-count |

These endpoints fetch data from the Swiggy Service using RestTemplate.

---

# 🔄 RestTemplate Communication

## Restaurant Management ➜ Swiggy

```
Restaurant Service
        │
        │ RestTemplate
        ▼
Swiggy Service

GET /api/items
GET /api/items/{id}
```

---

# 📁 Project Structure

```
src/main/java
│
├── controller
├── dto
├── exception
├── model
├── repository
└── service        (service interfaces and their implementations live together in this package)
```

The Restaurant Management Service additionally has a `config` package (`RestTemplateConfig`) that defines the shared `RestTemplate` bean used for outbound calls.

---

# 📦 Features

- Microservices Architecture
- RESTful APIs
- Independent Databases
- Spring Data JPA
- Bean Validation
- Global Exception Handling
- RestTemplate Communication
- CRUD Operations
- H2 Database
- Maven Project
- Layered Architecture

---

# ⚠ Known Limitations

- Deleting a restaurant permanently removes it (a hard delete via `RestaurantRepository`) — there is no separate "deactivate/reactivate" toggle for restaurants.
- H2 databases are in-memory and reset after application restart.
- No authentication or authorization implemented.
- Synchronous communication using RestTemplate only.
- The Delivery Management Service described in earlier versions of this project does not currently exist in the codebase.

---

# 👨‍💻 Author

**Vignesh J**

Spring Boot Microservices Project – Swiggy Clone
