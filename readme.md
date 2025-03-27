# ✈️ Airline Management System

## 🛠️ Overview
The **Airline Management System** is a microservices-based architecture designed to efficiently handle flight bookings, authentication, notifications, and API management. It ensures scalability, security, and seamless communication between different services using **RabbitMQ** for message queuing and **JWT** for authentication.

## 📌 Microservices Architecture
The system is built using **four microservices** and an **API Gateway**:

1. **Flight & Search Service** 🚀  
   - Manages flight details, schedules, and seat availability.
2. **Authentication Service** 🔒  
   - Handles user authentication, authorization, and role-based access control.
3. **Airline Ticket Booking Service** 🎟️  
   - Manages flight bookings, ticket pricing, and availability.
4. **Reminder Service** 📧  
   - Sends booking confirmation and reminder emails via **RabbitMQ**.
5. **API Gateway** 🌍  
   - Serves as a middleware for request handling, authentication, and rate limiting.

---

## 🚀 Tech Stack

| Component         | Technology Used          |
|------------------|------------------------|
| Backend         | **Node.js, Express.js** |
| Database        | **MySQL (via Sequelize ORM)** |
| Authentication  | **JWT, bcrypt** |
| Message Queue   | **RabbitMQ (via amqplib)** |
| Email Service   | **Nodemailer** |
| Job Scheduling  | **node-cron** |
| API Gateway     | **Express.js, Rate Limiting** |
| Deployment      | **AWS** |

---

## 🏗️ Microservices Breakdown

### 1️⃣ **Flight & Search Service**
- **Manages flight information** (e.g., schedules, availability).
- Provides **search functionality** for users.
- Ensures **seat count updates** in real-time.

### 2️⃣ **Authentication Service**
- Secure **user registration & login**.
- Implements **JWT-based authentication**.
- Provides **Role-Based Access Control (RBAC)**.

### 3️⃣ **Airline Ticket Booking Service**
- Handles **booking requests**.
- Validates **seat availability** before confirming bookings.
- Computes **total cost** dynamically.
- Publishes **notification messages** to RabbitMQ for email alerts.

### 4️⃣ **Reminder Service**
- Listens for booking events via **RabbitMQ**.
- Sends **email notifications** for bookings.
- Retries **failed email attempts** using **node-cron**.

### 5️⃣ **API Gateway (Middle-End)**
- Centralized API management.
- Implements **rate limiting** (5 requests per 2 minutes).
- Routes requests to respective services.
- Ensures **secure authentication** before forwarding requests.

---

## 📊 Database Schema
### **Bookings Table** (Airline Ticket Booking Service)
| Field      | Type          | Null | Key  | Default | Extra          |
|-----------|--------------|------|------|---------|----------------|
| id        | int          | NO   | PRI  | NULL    | auto_increment |
| userId    | int          | NO   |      | NULL    |                |
| flightId  | int          | NO   |      | NULL    |                |
| noOfSeats | int          | NO   |      | NULL    |                |
| totalCost | float        | NO   |      | NULL    |                |
| status    | varchar(255) | NO   |      | Pending |                |
| createdAt | datetime     | NO   |      | NULL    |                |
| updatedAt | datetime     | NO   |      | NULL    |                |

---

## 🔌 API Endpoints

### ✈️ **Flight & Search Service**
| Method | Endpoint                    | Description                        |
|--------|-----------------------------|------------------------------------|
| GET    | `/flights`                   | Fetch all available flights. |
| GET    | `/flights/:id`               | Get details of a specific flight. |

### 🔒 **Authentication Service**
| Method | Endpoint            | Description                     |
|--------|---------------------|---------------------------------|
| POST   | `/signUp`           | Registers a new user.          |
| POST   | `/signIn`           | Authenticates user and returns JWT. |
| GET    | `/isAuthenticated`  | Verifies if a user is authenticated. |
| GET    | `/isAdmin`          | Checks if the user has admin privileges. |
| DELETE | `/delete/:id`       | Deletes the user with specific ID. |

### 🎟️ **Airline Ticket Booking Service**
| Method | Endpoint           | Description                                      |
|--------|--------------------|--------------------------------------------------|
| POST   | `/bookings`        | Creates a new booking.                          |
| PATCH  | `/bookings/:id`    | Updates an existing booking.                    |
| POST   | `/publish`         | Sends a message to RabbitMQ for notification.   |

### 📧 **Reminder Service**
| Method | Endpoint           | Description                                  |
|--------|--------------------|----------------------------------------------|
| POST   | `/api/v1/tickets`  | Creates a new email notification.           |

### 🌍 **API Gateway (Proxy Routes)**
| Path                  | Target Service |
|----------------------|---------------|
| `/bookingservice`   | Booking Service (PORT3) |
| `/flightandsearchservice` | Flight and Search Service (PORT1) |
| `/authservice`      | Auth Service (PORT2) |

---

## 🔧 Setup & Deployment
### 📌 **Prerequisites**
- **Node.js** installed.
- **MySQL Database** setup.
- **RabbitMQ Server** running.

### ⚙️ **Installation**
Clone the repository:
```sh
  git clone https://github.com/your-repo/airline-management-system.git
  cd airline-management-system
```

Install dependencies:
```sh
  npm install
```

### 🌐 **Environment Variables**
Create a `.env` file and add:
```sh
PORT=3004  
PORT1=3000  
PORT2=3001  
PORT3=3002  
```

### 🚀 **Start the Microservices**
```sh
  npm start
```

---

## 🛡️ Security Features
✔ **JWT Authentication** – Secure user authentication with signed tokens.  
✔ **Role-Based Access Control (RBAC)** – Restricts API access based on roles.  
✔ **Rate Limiting** – Prevents API abuse by limiting request frequency.  
✔ **Message Queue** – Ensures reliable email notifications with **RabbitMQ**.  

---

## 🎯 Conclusion
The **Airline Management System** is a robust, **scalable**, and **efficient** platform designed to handle real-world airline operations. With **microservices architecture, message queuing, and secure authentication**, it ensures a seamless experience for both users and administrators.

🚀 **Now fully operational with RabbitMQ integration!** 🚀

