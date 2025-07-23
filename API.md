# 🔌 API Reference - Automated Calorie Counter

## Base URL

```
Production: https://api.caloriecounter.app/v1
Development: http://localhost:3000/api/v1
```

## Authentication

All API requests require authentication using JWT tokens.

### Headers

```http
Authorization: Bearer <your-jwt-token>
Content-Type: application/json
```

### Getting a Token

```http
POST /auth/login
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "secure-password"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

## Endpoints

### Authentication

#### Register New User

```http
POST /auth/register
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "secure-password",
  "name": "John Doe",
  "preferences": {
    "dailyCalorieTarget": 2000,
    "timezone": "America/New_York"
  }
}
```

**Response:**
```json
{
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "John Doe",
    "createdAt": "2024-01-20T10:00:00Z"
  }
}
```

#### Refresh Token

```http
POST /auth/refresh
```

**Request Body:**
```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

### Food Entries

#### List Food Entries

```http
GET /food/entries
```

**Query Parameters:**
- `date` (optional): ISO date string (default: today)
- `startDate` (optional): ISO date string for range
- `endDate` (optional): ISO date string for range
- `limit` (optional): Number of results (default: 50)
- `offset` (optional): Pagination offset

**Response:**
```json
{
  "entries": [
    {
      "id": "uuid",
      "name": "Chicken Sandwich",
      "calories": 450,
      "nutrition": {
        "protein": 30,
        "carbs": 45,
        "fat": 15,
        "fiber": 3
      },
      "loggedAt": "2024-01-20T12:30:00Z",
      "source": "manual",
      "metadata": {}
    }
  ],
  "total": 1250,
  "count": 3
}
```

#### Create Food Entry

```http
POST /food/entries
```

**Request Body:**
```json
{
  "description": "Grilled chicken sandwich with side salad",
  "loggedAt": "2024-01-20T12:30:00Z"
}
```

**Response:**
```json
{
  "entry": {
    "id": "uuid",
    "name": "Grilled chicken sandwich with side salad",
    "calories": 520,
    "nutrition": {
      "protein": 35,
      "carbs": 42,
      "fat": 18,
      "fiber": 5
    },
    "items": [
      {
        "name": "Grilled Chicken Sandwich",
        "calories": 450,
        "quantity": 1
      },
      {
        "name": "Side Salad",
        "calories": 70,
        "quantity": 1
      }
    ],
    "loggedAt": "2024-01-20T12:30:00Z",
    "source": "ai"
  }
}
```

#### Update Food Entry

```http
PATCH /food/entries/:id
```

**Request Body:**
```json
{
  "calories": 500,
  "name": "Grilled Chicken Sandwich (large)"
}
```

#### Delete Food Entry

```http
DELETE /food/entries/:id
```

### Food Recognition

#### Recognize Food from Photo

```http
POST /food/recognize/photo
```

**Request Body (multipart/form-data):**
- `image`: Image file (JPEG, PNG)
- `includeNutrition`: boolean (optional, default: true)

**Response:**
```json
{
  "items": [
    {
      "name": "Pizza",
      "confidence": 0.95,
      "calories": 285,
      "portion": "1 slice",
      "boundingBox": {
        "x": 100,
        "y": 50,
        "width": 200,
        "height": 150
      }
    }
  ],
  "totalCalories": 570,
  "imageId": "uuid"
}
```

#### Recognize Food from Voice

```http
POST /food/recognize/voice
```

**Request Body:**
```json
{
  "audio": "base64-encoded-audio",
  "format": "webm"
}
```

**Alternative - Text Input:**
```json
{
  "text": "I had two eggs and toast for breakfast"
}
```

**Response:**
```json
{
  "transcript": "I had two eggs and toast for breakfast",
  "items": [
    {
      "name": "Eggs",
      "quantity": 2,
      "unit": "large",
      "calories": 140
    },
    {
      "name": "Toast",
      "quantity": 1,
      "unit": "slice",
      "calories": 70
    }
  ],
  "totalCalories": 210
}
```

#### Scan Receipt

```http
POST /food/recognize/receipt
```

**Request Body (multipart/form-data):**
- `image`: Receipt image
- `restaurant`: Restaurant name (optional)

**Response:**
```json
{
  "restaurant": "Chipotle",
  "items": [
    {
      "name": "Chicken Burrito Bowl",
      "calories": 625,
      "price": 10.95
    }
  ],
  "totalCalories": 625,
  "confidence": 0.92
}
```

### Nutrition Database

#### Search Foods

```http
GET /nutrition/search
```

**Query Parameters:**
- `q`: Search query (required)
- `limit`: Number of results (default: 20)
- `category`: Food category filter

**Response:**
```json
{
  "results": [
    {
      "id": "food-id",
      "name": "Apple",
      "brand": null,
      "calories": 95,
      "serving": {
        "size": 1,
        "unit": "medium",
        "grams": 182
      },
      "nutrition": {
        "protein": 0.5,
        "carbs": 25,
        "fat": 0.3,
        "fiber": 4.4
      }
    }
  ]
}
```

#### Get Food Details

```http
GET /nutrition/details/:id
```

**Response:**
```json
{
  "food": {
    "id": "food-id",
    "name": "Apple",
    "calories": 95,
    "servings": [
      {
        "size": 1,
        "unit": "medium",
        "grams": 182,
        "calories": 95
      },
      {
        "size": 100,
        "unit": "grams",
        "calories": 52
      }
    ],
    "nutrition": {
      "protein": 0.5,
      "carbs": 25,
      "fat": 0.3,
      "fiber": 4.4,
      "sugar": 19,
      "sodium": 2,
      "vitamins": {
        "vitaminC": 14,
        "vitaminA": 2
      },
      "minerals": {
        "calcium": 1,
        "iron": 1
      }
    }
  }
}
```

### Analytics

#### Get Daily Summary

```http
GET /analytics/daily
```

**Query Parameters:**
- `date`: ISO date string (default: today)

**Response:**
```json
{
  "date": "2024-01-20",
  "calories": {
    "consumed": 1850,
    "target": 2000,
    "remaining": 150
  },
  "nutrition": {
    "protein": 95,
    "carbs": 220,
    "fat": 65,
    "fiber": 28
  },
  "meals": {
    "breakfast": 450,
    "lunch": 650,
    "dinner": 580,
    "snacks": 170
  }
}
```

#### Get Trends

```http
GET /analytics/trends
```

**Query Parameters:**
- `period`: "week" | "month" | "quarter" | "year"
- `metric`: "calories" | "weight" | "nutrition"

**Response:**
```json
{
  "period": "week",
  "data": [
    {
      "date": "2024-01-14",
      "calories": 1950,
      "target": 2000
    },
    {
      "date": "2024-01-15",
      "calories": 2100,
      "target": 2000
    }
  ],
  "average": 2025,
  "trend": "stable"
}
```

#### Get Insights

```http
GET /analytics/insights
```

**Response:**
```json
{
  "insights": [
    {
      "type": "pattern",
      "title": "Weekend Overeating",
      "description": "You tend to consume 25% more calories on weekends",
      "priority": "medium",
      "suggestions": [
        "Plan weekend meals in advance",
        "Increase activity on weekends"
      ]
    },
    {
      "type": "achievement",
      "title": "7-Day Logging Streak!",
      "description": "You've logged meals for 7 days straight",
      "priority": "low"
    }
  ]
}
```

### User Management

#### Get User Profile

```http
GET /user/profile
```

**Response:**
```json
{
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "John Doe",
    "profile": {
      "age": 30,
      "height": 180,
      "weight": 75,
      "activityLevel": "moderate",
      "goals": {
        "dailyCalories": 2000,
        "weightGoal": "maintain"
      }
    },
    "preferences": {
      "units": "metric",
      "theme": "dark",
      "notifications": {
        "mealReminders": true,
        "dailySummary": true
      }
    }
  }
}
```

#### Update User Profile

```http
PATCH /user/profile
```

**Request Body:**
```json
{
  "profile": {
    "weight": 74,
    "activityLevel": "high"
  },
  "preferences": {
    "theme": "light"
  }
}
```

### Integrations

#### Connect Wearable Device

```http
POST /integrations/wearables/connect
```

**Request Body:**
```json
{
  "provider": "fitbit",
  "authCode": "oauth-auth-code"
}
```

#### Sync Meal Delivery

```http
POST /integrations/meal-delivery/sync
```

**Request Body:**
```json
{
  "provider": "ubereats",
  "orderId": "order-123"
}
```

## Error Responses

All endpoints may return these error responses:

### 400 Bad Request

```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Invalid request parameters",
    "details": {
      "field": "email",
      "reason": "Invalid email format"
    }
  }
}
```

### 401 Unauthorized

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Authentication required"
  }
}
```

### 403 Forbidden

```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "Insufficient permissions"
  }
}
```

### 404 Not Found

```json
{
  "error": {
    "code": "NOT_FOUND",
    "message": "Resource not found"
  }
}
```

### 429 Too Many Requests

```json
{
  "error": {
    "code": "RATE_LIMITED",
    "message": "Too many requests",
    "retryAfter": 60
  }
}
```

### 500 Internal Server Error

```json
{
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "An unexpected error occurred",
    "requestId": "req-123"
  }
}
```

## Rate Limiting

- **Anonymous**: 10 requests per minute
- **Authenticated**: 100 requests per minute
- **Premium**: 1000 requests per minute

Rate limit information is included in response headers:

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1642694400
```

## Webhooks

Configure webhooks to receive real-time updates:

### Event Types

- `food.entry.created`
- `food.entry.updated`
- `food.entry.deleted`
- `goal.achieved`
- `insight.generated`

### Webhook Payload

```json
{
  "id": "webhook-event-id",
  "type": "food.entry.created",
  "timestamp": "2024-01-20T12:30:00Z",
  "data": {
    "entry": {
      "id": "uuid",
      "calories": 450
    }
  }
}
```

### Webhook Security

Verify webhook signatures using HMAC-SHA256:

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const hash = crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');
  
  return hash === signature;
}
```

## SDKs

Official SDKs are available for:

- JavaScript/TypeScript
- Python
- Swift (iOS)
- Kotlin (Android)
- Go

### JavaScript Example

```javascript
import { CalorieCounterAPI } from '@caloriecounter/sdk';

const api = new CalorieCounterAPI({
  token: 'your-api-token'
});

// Log a meal
const entry = await api.food.createEntry({
  description: 'Chicken salad and apple juice'
});

// Get daily summary
const summary = await api.analytics.getDailySummary();
```

## API Versioning

The API uses URL versioning. The current version is `v1`.

When breaking changes are introduced:
1. New version will be released (e.g., `v2`)
2. Previous version supported for 12 months
3. Deprecation notices sent via email and headers

## Support

- API Status: https://status.caloriecounter.app
- Documentation: https://docs.caloriecounter.app
- Support: api-support@caloriecounter.app
- Community: https://community.caloriecounter.app