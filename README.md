# FinEdge - Financial Management API

A comprehensive RESTful API for personal financial management, built with Node.js and Express. Track transactions, analyze spending patterns, and gain insights into your financial health.

## Table of Contents
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [API Documentation](#api-documentation)
- [Error Handling](#error-handling)
- [API Versioning](#api-versioning)
- [Project Structure](#project-structure)
- [Development Team](#development-team)

---

## Features

- **User Management**: Complete CRUD operations for user accounts
- **Transaction Tracking**: Record and manage income and expense transactions
- **Financial Analytics**: Comprehensive spending insights and summaries
- **Category Analysis**: Track spending by category with detailed breakdowns
- **AI-Powered Insights**: Get personalized financial recommendations
- **Flexible Filtering**: Filter data by date ranges, categories, and transaction types
- **Centralized Error Handling**: Consistent error responses across all endpoints
- **Request Logging**: Track all API requests and responses
- **Input Validation**: Robust validation middleware for data integrity

---

## Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js 5.2.1
- **Data Storage**: JSON files
- **Middleware**: CORS, Custom error handling, logging, and validation

---

## Getting Started

### Prerequisites
- Node.js (v14 or higher)
- npm or yarn

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd finedge
```

2. Install dependencies:
```bash
npm install
```

3. Start the server:
```bash
node src/app.js
```

The API will be available at `http://localhost:3000`

---

## API Documentation

### Base URL
```
http://localhost:3000/api/v1
```

### Users API

#### Get All Users
```http
GET /api/v1/users
```

#### Get User by ID
```http
GET /api/v1/users/:id
```

#### Create User
```http
POST /api/v1/users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
```

#### Update User
```http
PUT /api/v1/users/:id
Content-Type: application/json

{
  "name": "John Updated",
  "email": "john.updated@example.com"
}
```

#### Delete User
```http
DELETE /api/v1/users/:id
```

---

### Transactions API

#### Get All Transactions
```http
GET /api/v1/transactions
```

#### Get Transaction by ID
```http
GET /api/v1/transactions/:id
```

#### Get Transactions by User
```http
GET /api/v1/transactions/user/:userId
```

#### Create Transaction
```http
POST /api/v1/transactions
Content-Type: application/json

{
  "userId": 1,
  "type": "expense",
  "category": "food",
  "amount": 50.00,
  "description": "Grocery shopping",
  "date": "2024-01-15"
}
```

#### Update Transaction
```http
PUT /api/v1/transactions/:id
Content-Type: application/json

{
  "amount": 55.00,
  "description": "Updated description"
}
```

#### Delete Transaction
```http
DELETE /api/v1/transactions/:id
```

---

### Analytics API

#### Get User Financial Summary
```http
GET /api/v1/analytics/user/:userId/summary?startDate=2024-01-01&endDate=2024-12-31
```

**Response:**
```json
{
  "success": true,
  "data": {
    "userId": 1,
    "period": {
      "startDate": "2024-01-01",
      "endDate": "2024-12-31"
    },
    "summary": {
      "totalIncome": 5000.00,
      "totalExpense": 3500.00,
      "balance": 1500.00,
      "transactionCount": 45,
      "incomeCount": 5,
      "expenseCount": 40
    },
    "categoryBreakdown": {
      "food": { "count": 15, "total": 1200.00 }
    },
    "monthlyBreakdown": {
      "2024-01": { "income": 2000, "expense": 1200, "count": 15 }
    },
    "topSpendingCategory": {
      "category": "food",
      "amount": 1200.00,
      "count": 15
    }
  }
}
```

#### Get User Spending Insights
```http
GET /api/v1/analytics/user/:userId/insights?startDate=2024-01-01&endDate=2024-12-31
```

**Response:**
```json
{
  "success": true,
  "data": {
    "userId": 1,
    "summary": {
      "totalIncome": 5000.00,
      "totalExpense": 3500.00,
      "balance": 1500.00
    },
    "insights": [
      {
        "type": "success",
        "message": "Great! You have a positive balance of $1500.00."
      }
    ],
    "recommendations": [
      "Review and reduce expenses in non-essential categories"
    ],
    "topCategories": [
      {
        "category": "food",
        "amount": 1200.00,
        "percentage": "34.3"
      }
    ]
  }
}
```

#### Get Category Breakdown
```http
GET /api/v1/analytics/user/:userId/categories?startDate=2024-01-01&endDate=2024-12-31
```

#### Get Transaction Statistics
```http
GET /api/v1/analytics/stats?userId=1&category=food&type=expense
```

**Query Parameters:**
- `userId` (optional) - Filter by user ID
- `startDate` (optional) - Start date (YYYY-MM-DD)
- `endDate` (optional) - End date (YYYY-MM-DD)
- `category` (optional) - Filter by category
- `type` (optional) - Filter by type (income/expense)

---

## Error Handling

### Error Response Format

All errors follow a consistent format:

**Operational Error (4xx):**
```json
{
  "success": false,
  "status": "fail",
  "error": {
    "message": "User not found"
  }
}
```

**Server Error (5xx):**
```json
{
  "success": false,
  "status": "error",
  "error": {
    "message": "Something went wrong!"
  }
}
```

### Common Status Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request (validation errors)
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Internal Server Error

### Error Handling Architecture

The project uses a centralized error handling system:

1. **AppError Class** (`src/utils/AppError.js`): Custom error class for operational errors
2. **AsyncHandler** (`src/utils/asyncHandler.js`): Wraps async functions to catch errors automatically
3. **Error Middleware** (`src/middleware/errorHandler.js`): Centralized error processing

**Example Usage:**
```javascript
const asyncHandler = require('./utils/asyncHandler');
const AppError = require('./utils/AppError');

const getUser = asyncHandler(async (req, res) => {
  const user = await userService.getUserById(req.params.id);
  if (!user) {
    throw new AppError('User not found', 404);
  }
  res.status(200).json({ success: true, data: user });
});
```

---

## API Versioning

### Current Version: v1

The API uses URL path versioning for backward compatibility:

```
/api/v1/users          - Version 1 User APIs
/api/v1/transactions   - Version 1 Transaction APIs
/api/v1/analytics      - Version 1 Analytics APIs
```

### Backward Compatibility

Non-versioned routes automatically redirect to v1:
- `/api/users` → `/api/v1/users`
- `/api/transactions` → `/api/v1/transactions`

### Version Migration

When introducing breaking changes:
1. Create new version folder (e.g., `src/routes/v2`)
2. Copy and modify routes as needed
3. Update `app.js` to register new version
4. Maintain old version for at least 6 months
5. Provide migration guide for clients

### When to Create a New Version
- Breaking changes to request/response formats
- Removing endpoints
- Changing authentication/authorization
- Major refactoring of business logic

### When NOT to Create a New Version
- Adding new endpoints
- Adding optional fields
- Bug fixes
- Performance improvements

---

## Project Structure

```
finedge/
├── src/
│   ├── app.js                          # Main application entry point
│   ├── controllers/                    # Request handlers
│   │   ├── userController.js
│   │   ├── transactionController.js
│   │   └── analyticsController.js
│   ├── routes/                         # API route definitions
│   │   └── v1/                         # Version 1 routes
│   ├── services/                       # Business logic layer
│   │   ├── userService.js
│   │   └── transactionService.js
│   ├── models/                         # Data access layer
│   │   ├── userModel.js
│   │   └── transactionModel.js
│   ├── middleware/                     # Custom middleware
│   │   ├── errorHandler.js
│   │   ├── logger.js
│   │   └── validator.js
│   ├── utils/                          # Utility functions
│   │   ├── AppError.js
│   │   ├── asyncHandler.js
│   │   ├── analytics.js
│   │   └── aiHelper.js
│   └── data/                           # JSON data storage
│       ├── users.json
│       └── transactions.json
├── package.json
├── .gitignore
└── README.md
```

---

## Development Team

### Module Allocation

#### Member 1: User APIs Module
- User routes and controllers
- User service and model
- CRUD operations for users
- Files: `userRoutes.js`, `userController.js`, `userService.js`, `userModel.js`

#### Member 2: Transaction APIs Module
- Transaction routes and controllers
- Transaction service and model
- CRUD operations for transactions
- User-specific transaction queries
- Files: `transactionRoutes.js`, `transactionController.js`, `transactionService.js`, `transactionModel.js`

#### Member 3: Middleware & Utils Module
- Error handling middleware
- Request/response logging
- Input validation
- Files: `errorHandler.js`, `logger.js`, `validator.js`

#### Member 4: Analytics & Documentation Module
- Financial analytics and summaries
- Spending insights and reports
- API documentation
- Postman collection
- Files: `analytics.js`, `aiHelper.js`, `FinEdge_API.postman_collection.json`

---

## Testing

Import the Postman collection (`FinEdge_API.postman_collection.json`) to test all API endpoints.

---

## Best Practices

### Controllers
- Use `asyncHandler` wrapper for all async functions
- Throw `AppError` for operational errors
- Keep controllers thin - delegate logic to services
- Return consistent response formats

### Services
- Contain all business logic
- Validate data before processing
- Throw errors for invalid operations
- Keep functions focused and testable

### Models
- Handle all data access operations
- Read/write to JSON files
- Generate unique IDs
- Return null for not found items

### Error Handling
- Use `AppError` for expected errors
- Let errors bubble up from services
- Don't use try-catch in controllers
- Provide descriptive error messages

---

## License

ISC

---

## Contributing

1. Follow the module allocation guidelines
2. Use consistent error handling patterns
3. Validate all inputs using the validator middleware
4. Update Postman collection when adding new endpoints
5. Document all API changes

---

## Support

For issues and questions, please refer to the individual documentation files:
- `ANALYTICS_API.md` - Analytics endpoints details
- `API_VERSIONING.md` - Versioning guidelines
- `ERROR_HANDLING.md` - Error handling patterns
- `MEMBER_ALLOCATION.md` - Team responsibilities
