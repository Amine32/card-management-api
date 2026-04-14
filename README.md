# Card Management API

REST API for managing card accounts — issuance, activation, transactions, and balance management.

![Java](https://img.shields.io/badge/Java-11-orange?style=flat&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-2.6.2-brightgreen?style=flat&logo=springboot)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-blue?style=flat&logo=postgresql)
![Spring Security](https://img.shields.io/badge/Spring_Security-enabled-brightgreen?style=flat&logo=springsecurity)
![Swagger](https://img.shields.io/badge/Swagger-OpenAPI_3-85EA2D?style=flat&logo=swagger)

---

## Architecture

```
Client
  │
  ▼
Controller Layer  (/api/accounts, /api/cards, /api/transactions)
  │
  ▼
Service Layer     (Business logic, AES encryption, validation)
  │
  ▼
Repository Layer  (Spring Data JPA)
  │
  ▼
PostgreSQL        (schema: cms)
```

---

## API Endpoints

### Accounts — `/api/accounts`

| Method   | Path                                    | Description                              |
|----------|-----------------------------------------|------------------------------------------|
| `GET`    | `/api/accounts/all`                     | Get all accounts (paginated)             |
| `GET`    | `/api/accounts/{accountId}`             | Get account by ID                        |
| `POST`   | `/api/accounts`                         | Create a new account                     |
| `PATCH`  | `/api/accounts/invertActivation/{accountId}` | Toggle account active/inactive status |
| `DELETE` | `/api/accounts/{accountId}`             | Delete an account                        |

### Cards — `/api/cards`

| Method   | Path                                    | Description                              |
|----------|-----------------------------------------|------------------------------------------|
| `GET`    | `/api/cards/all`                        | Get all cards (paginated)                |
| `GET`    | `/api/cards/{cardId}`                   | Get card by ID                           |
| `POST`   | `/api/cards`                            | Issue a new card for an account          |
| `PATCH`  | `/api/cards/renew/{cardId}`             | Renew card expiry date                   |
| `PATCH`  | `/api/cards/invertActivation/{cardId}`  | Toggle card active/inactive status       |
| `DELETE` | `/api/cards/{cardId}`                   | Delete a card                            |

### Transactions — `/api/transactions`

| Method | Path                                        | Description                              |
|--------|---------------------------------------------|------------------------------------------|
| `GET`  | `/api/transactions/byCard/{cardId}`         | Get transactions for a card (paginated)  |
| `GET`  | `/api/transactions/{transactionId}`         | Get transaction by ID                    |
| `POST` | `/api/transactions`                         | Create a new transaction (credit/debit)  |

Full interactive documentation available at `http://localhost:8080/swagger-ui.html` once the app is running.

---

## Getting Started with Docker

### Prerequisites

- Docker and Docker Compose installed

### Run

```bash
git clone https://github.com/your-username/card-management-api.git
cd card-management-api
docker compose up --build
```

The API will be available at `http://localhost:8080`.

---

## Example Requests

### Create an account

```bash
curl -X POST http://localhost:8080/api/accounts \
  -H "Content-Type: application/json"
```

### Issue a card

```bash
curl -X POST http://localhost:8080/api/cards \
  -H "Content-Type: application/json" \
  -d '{"accountId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"}'
```

### Post a debit transaction

```bash
curl -X POST http://localhost:8080/api/transactions \
  -H "Content-Type: application/json" \
  -d '{
    "cardId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "amount": 50.00,
    "transactionType": "D"
  }'
```

> Transaction types: `C` (credit — funds added to balance), `D` (debit — funds deducted from balance)

---

## Project Structure

```
card-management-api/
├── src/
│   └── main/
│       ├── java/com/example/demo/
│       │   ├── common/          # Shared utilities (Enums, DtoConverter, EntityFetcher)
│       │   ├── config/          # Security and OpenAPI configuration
│       │   ├── controller/      # REST controllers (Account, Card, Transaction)
│       │   ├── dto/             # Request and response DTOs
│       │   ├── exception/       # Custom exception classes
│       │   ├── model/           # JPA entities (Account, Card, Transaction)
│       │   ├── repository/      # Spring Data JPA repositories
│       │   ├── security/        # AES encryption service
│       │   └── service/         # Business logic (interfaces + implementations)
│       └── resources/
│           └── application.properties
├── Dockerfile
├── docker-compose.yml
├── build.gradle
└── README.md
```

---

## License

MIT — see [LICENSE](LICENSE)
