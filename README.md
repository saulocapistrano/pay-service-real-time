# **Technical Challenge: Real-Time Payment Settlement System**

### **Business Context**
You have been hired to develop the core of a **Real-Time Payment Settlement System** for a digital bank. The system must process financial transactions between different accounts, ensuring consistency, traceability, and high availability.

### **Functional Requirements**

1. **Transaction Processing**
   - Create, validate, and settle transactions between accounts
   - Support different transaction types (PIX, TED, DOC)
   - Validate sufficient balance in the source account
   - Ensure transactions are not processed duplicate

2. **Real-Time Reconciliation**
   - System must automatically reconcile transactions
   - Maintain transaction status history (PENDING, PROCESSING, SETTLED, FAILED)

3. **Limits and Compliance**
   - Apply transactional limits by operation type
   - Maintain complete audit trail for regulatory compliance

### **Non-Functional Requirements**
- **Availability:** 99.9%
- **Latency:** < 100ms for settlement
- **Consistency:** Strong for critical operations
- **Traceability:** Structured logging with correlation ID
- **Resilience:** Partial failure tolerance

---

## **Technical Challenge**

### **Part 1: Architecture and Design (Documentation)**

**1.1. Architecture Diagram**
Design the system architecture considering:
- Microservices vs Modular Monolith
- Persistence strategy (possible CQRS?)
- Synchronous vs asynchronous communication
- Cache and optimizations

**1.2. Data Design**
Model the main entities:
- `Account`
- `Transaction`
- `TransactionLimit`
- `AuditLog`

**1.3. API Design**
Design RESTful APIs:
```java
POST /api/v1/transactions - Create transaction
GET /api/v1/transactions/{id} - Get transaction
GET /api/v1/accounts/{accountId}/transactions - History
POST /api/v1/accounts/{accountId}/reconciliation - Reconciliation
```

### **Part 2: Implementation (Code)**

**2.1. Core Domain**
Implement rich domain entities with validations:

```java
// Example skeleton
@Entity
public class Transaction {
    private TransactionId id;
    private AccountId sourceAccount;
    private AccountId targetAccount;
    private MonetaryAmount amount;
    private TransactionType type;
    private TransactionStatus status;
    private LocalDateTime createdAt;
    
    public Result<Transaction> process(Account source, Account target) {
        // Implement business logic
    }
}
```

**2.2. Service Layer**
Implement transaction service with:
- Balance validation
- Limit application
- Event publishing

**2.3. Kafka Integration**
- Producer for processed transaction events
- Consumer for asynchronous reconciliation
- Dead Letter Queue for failure retries

**2.4. Redis Cache**
- Account balance cache (eventual consistency OK)
- Transaction limits cache
- Invalidation strategy

**2.5. Testing**
```java
@Test
void shouldProcessTransactionSuccessfully() {
    // Complete test with Mockito and JUnit 5
    // Include integration tests with @Testcontainers
}

@Test
void shouldFailWhenInsufficientBalance() {
    // Failure scenario test
}
```

### **Part 3: Infrastructure and DevOps**

**3.1. Docker & Docker Compose**
Create `docker-compose.yml` with:
- PostgreSQL (2 instances for dev/test)
- Redis
- Kafka + Zookeeper
- Spring Boot application

**3.2. Terraform**
Sketch Terraform module for:
- Security groups
- RDS PostgreSQL
- Elasticache Redis
- MSK Kafka

**3.3. Liquibase**
Create changelogs for:
- Initial tables
- Performance indexes
- Reference data (transaction types)

**3.4. Observability**
Configure:
- Custom metrics in DataDog (latency, success rate)
- Structured logs with MDC
- Custom health checks

---

## **Evaluation Criteria**

### **Senior Level Expectations**

| Category | Weight | Criteria |
|----------|--------|----------|
| **Design & Architecture** | 30% | - Clean Architecture<br>- Domain-Driven Design<br>- Appropriate patterns<br>- Justified decisions |
| **Code Quality** | 25% | - SOLID principles<br>- Testability<br>- Error handling<br>- Clean code |
| **Infra & DevOps** | 20% | - IaC (Terraform)<br>- Containerization<br>- Database migration<br>- Observability |
| **Resilience** | 15% | - Retry patterns<br>- Circuit breaker<br>- Consistent caching<br>- DLQ handling |
| **Performance** | 10% | - Appropriate optimizations<br>- Query efficiency<br>- Cache strategy |

### **Senior+ Differentiators**
- ✅ Idempotency keys implementation
- ✅ Strategy pattern for different transaction types
- ✅ Event sourcing for auditing
- ✅ Outbox pattern for consistency
- ✅ Feature flags for gradual deployment
- ✅ Contract testing for APIs

---

## **Expected Delivery**

1. **Git Repository** with:
   - Complete application code
   - Dockerfile and docker-compose.yml
   - Liquibase scripts
   - Terraform modules (sketch)
   - Postman/Insomnia collection

2. **Documentation** containing:
   - Architecture decisions
   - Diagrams (architecture, sequence, data)
   - Local deployment instructions
   - Discussed trade-offs

3. **Presentation** (if applicable):
   - 15-minute demo
   - Technical decisions explanation
   - Performance metrics

---

## **Bonus: Complex Scenarios**

To evaluate real experience:
```java
// How would you handle:
- Concurrent transactions on the same account?
- Rollback in distributed transactions?
- Event replay after failure?
- Schema migration without downtime?
```

This challenge tests technical depth, architectural thinking, and ability to build robust financial systems - exactly what's expected from a senior developer in the mentioned stack.

**Good luck!** 🚀
