# 🏗️ Architecture Guide - Automated Calorie Counter

## 📐 System Architecture Overview

The Automated Calorie Counter follows a modern, scalable architecture designed for reliability, performance, and extensibility.

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend Apps                         │
├─────────────┬─────────────┬─────────────┬─────────────────┤
│   Web App   │   iOS App   │ Android App │   Watch Apps    │
│   (React)   │   (Swift)   │  (Kotlin)   │  (WatchKit)     │
└─────────────┴─────────────┴─────────────┴─────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      API Gateway                             │
│                  (AWS API Gateway)                           │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┴─────────────────────┐
        ▼                                           ▼
┌──────────────────┐                      ┌──────────────────┐
│  Authentication  │                      │  Load Balancer   │
│     Service      │                      │   (AWS ALB)      │
└──────────────────┘                      └──────────────────┘
                                                   │
                    ┌──────────────────────────────┴──────────────────────────────┐
                    ▼                              ▼                              ▼
         ┌──────────────────┐          ┌──────────────────┐          ┌──────────────────┐
         │   Core Service   │          │   ML Service     │          │ Integration Hub   │
         │   (Node.js)      │          │   (Python)       │          │   (Node.js)       │
         └──────────────────┘          └──────────────────┘          └──────────────────┘
                    │                              │                              │
                    ▼                              ▼                              ▼
         ┌──────────────────┐          ┌──────────────────┐          ┌──────────────────┐
         │   PostgreSQL     │          │   Redis Cache    │          │  S3 Storage      │
         │   (Primary DB)   │          │   (Sessions)     │          │  (Images)        │
         └──────────────────┘          └──────────────────┘          └──────────────────┘
```

## 🔧 Technology Stack

### Frontend
- **Web**: React 18, TypeScript, Tailwind CSS, Vite
- **iOS**: Swift 5, SwiftUI, Combine
- **Android**: Kotlin, Jetpack Compose, Coroutines
- **State Management**: Redux Toolkit (web), SwiftUI State (iOS), ViewModel (Android)
- **API Client**: Axios with interceptors, Native HTTP clients

### Backend
- **API Layer**: Node.js, Express, TypeScript
- **ML Service**: Python, FastAPI, TensorFlow/PyTorch
- **Authentication**: JWT, OAuth 2.0, Passport.js
- **Real-time**: WebSockets (Socket.io)
- **Message Queue**: RabbitMQ / AWS SQS
- **Caching**: Redis

### Infrastructure
- **Cloud Provider**: AWS (primary), with multi-cloud ready architecture
- **Container**: Docker, Kubernetes (EKS)
- **CI/CD**: GitHub Actions, ArgoCD
- **Monitoring**: Prometheus, Grafana, ELK Stack
- **CDN**: CloudFlare

### Data Storage
- **Primary Database**: PostgreSQL 14+
- **Caching**: Redis 6+
- **Object Storage**: AWS S3
- **Search**: Elasticsearch
- **Analytics**: ClickHouse

### Third-Party Services
- **AI/ML**: OpenAI API, Google Vision AI, AWS Rekognition
- **Nutrition Data**: Nutritionix API, USDA FoodData Central
- **Payments**: Stripe
- **Email**: SendGrid
- **SMS**: Twilio

## 🏛️ Architectural Patterns

### 1. Microservices Architecture
Services are loosely coupled and independently deployable:

```
core-service/
├── user-management/
├── calorie-tracking/
├── goal-management/
└── reporting/

ml-service/
├── food-recognition/
├── portion-estimation/
├── predictive-analysis/
└── pattern-learning/

integration-hub/
├── restaurant-apis/
├── wearable-sync/
├── health-apps/
└── meal-delivery/
```

### 2. Event-Driven Architecture
Key events flow through the system:

```
UserFoodLogged → CalculateNutrition → UpdateDailyTotal → CheckGoals → SendNotifications
PhotoUploaded → ProcessImage → IdentifyFood → EstimateCalories → ConfirmWithUser
```

### 3. CQRS Pattern
Separate read and write models for optimal performance:

- **Write Model**: Optimized for fast data ingestion
- **Read Model**: Denormalized views for analytics and reporting

### 4. API-First Design
All functionality exposed through well-documented REST APIs:

```
/api/v1/
├── /auth
│   ├── POST /login
│   ├── POST /register
│   └── POST /refresh
├── /food
│   ├── GET /entries
│   ├── POST /entries
│   ├── POST /recognize/photo
│   └── POST /recognize/voice
├── /nutrition
│   ├── GET /search
│   └── GET /details/:id
└── /analytics
    ├── GET /trends
    └── GET /insights
```

## 💾 Data Architecture

### Database Schema (Simplified)

```sql
-- Users table
CREATE TABLE users (
    id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE,
    created_at TIMESTAMP,
    settings JSONB
);

-- Food entries table
CREATE TABLE food_entries (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    name VARCHAR(255),
    calories INTEGER,
    nutrition JSONB,
    logged_at TIMESTAMP,
    source VARCHAR(50), -- 'manual', 'photo', 'voice', 'receipt'
    metadata JSONB
);

-- Goals table
CREATE TABLE goals (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    type VARCHAR(50),
    target_value INTEGER,
    period VARCHAR(20),
    created_at TIMESTAMP
);
```

### Caching Strategy

1. **User Session Cache** (Redis)
   - TTL: 30 minutes
   - Contains: User preferences, current day stats

2. **Food Database Cache** (Redis)
   - TTL: 24 hours
   - Contains: Frequently searched foods

3. **Analytics Cache** (Redis)
   - TTL: 1 hour
   - Contains: Pre-computed analytics

## 🔐 Security Architecture

### Authentication & Authorization
- **JWT tokens** with refresh token rotation
- **OAuth 2.0** for social logins
- **Role-based access control** (RBAC)
- **API rate limiting** per user/IP

### Data Security
- **Encryption at rest** (AES-256)
- **Encryption in transit** (TLS 1.3)
- **Field-level encryption** for sensitive data
- **Regular security audits**

### Privacy Compliance
- **Data minimization** principles
- **User consent management**
- **Data retention policies**
- **Right to deletion** implementation

## 🚀 Scalability Considerations

### Horizontal Scaling
- **Stateless services** for easy scaling
- **Database read replicas** for read-heavy workloads
- **Sharding strategy** for user data
- **Auto-scaling policies** based on metrics

### Performance Optimization
- **CDN** for static assets
- **Image optimization** pipeline
- **API response caching**
- **Database query optimization**
- **Connection pooling**

### High Availability
- **Multi-AZ deployment**
- **Database failover**
- **Health checks**
- **Circuit breakers**
- **Graceful degradation**

## 🔄 CI/CD Pipeline

```yaml
# Simplified GitHub Actions workflow
name: Deploy
on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - run: npm test
      - run: npm run lint
      
  build:
    needs: test
    steps:
      - run: docker build -t app .
      - run: docker push registry/app
      
  deploy:
    needs: build
    steps:
      - run: kubectl apply -f k8s/
```

## 📊 Monitoring & Observability

### Metrics Collection
- **Application metrics**: Response times, error rates, throughput
- **Infrastructure metrics**: CPU, memory, disk, network
- **Business metrics**: User signups, feature usage, conversion

### Logging Strategy
- **Structured logging** (JSON format)
- **Centralized log aggregation** (ELK Stack)
- **Log retention policies**
- **Error tracking** (Sentry)

### Distributed Tracing
- **Request tracing** across microservices
- **Performance bottleneck identification**
- **Correlation IDs** for debugging

## 🔌 Integration Architecture

### External API Integration Pattern

```typescript
interface FoodRecognitionService {
  recognizeFood(image: Buffer): Promise<FoodItem[]>
}

class GoogleVisionAdapter implements FoodRecognitionService {
  async recognizeFood(image: Buffer): Promise<FoodItem[]> {
    // Implementation
  }
}

class FoodRecognitionFacade {
  constructor(
    private primary: FoodRecognitionService,
    private fallback: FoodRecognitionService
  ) {}
  
  async recognize(image: Buffer): Promise<FoodItem[]> {
    try {
      return await this.primary.recognizeFood(image)
    } catch (error) {
      return await this.fallback.recognizeFood(image)
    }
  }
}
```

### Webhook Architecture
- **Inbound webhooks** for meal delivery updates
- **Outbound webhooks** for third-party integrations
- **Webhook security** with HMAC signatures
- **Retry logic** with exponential backoff

## 🧪 Testing Strategy

### Testing Pyramid
1. **Unit Tests** (70%)
   - Jest for JavaScript/TypeScript
   - XCTest for iOS
   - JUnit for Android

2. **Integration Tests** (20%)
   - API endpoint testing
   - Database integration tests
   - External service mocking

3. **E2E Tests** (10%)
   - Cypress for web
   - XCUITest for iOS
   - Espresso for Android

### Performance Testing
- **Load testing** with k6
- **Stress testing** for peak loads
- **Capacity planning** based on metrics

## 🚨 Disaster Recovery

### Backup Strategy
- **Database backups**: Daily automated backups with point-in-time recovery
- **Code backups**: Git repository mirroring
- **Configuration backups**: Infrastructure as Code

### Recovery Procedures
- **RTO** (Recovery Time Objective): 1 hour
- **RPO** (Recovery Point Objective): 15 minutes
- **Automated failover** for critical services
- **Disaster recovery drills** quarterly

## 📈 Future Architecture Considerations

### AI/ML Pipeline
- **Model training pipeline** for custom models
- **A/B testing framework** for model improvements
- **Edge computing** for offline capabilities

### Blockchain Integration (Future)
- **Decentralized data storage** options
- **Smart contracts** for reward systems
- **Data ownership** verification

### IoT Integration
- **Smart scale** integration
- **Kitchen appliance** connectivity
- **Restaurant POS** integration

---

This architecture is designed to support millions of users while maintaining high performance, reliability, and security. It provides a solid foundation for current features while being flexible enough to accommodate future innovations.