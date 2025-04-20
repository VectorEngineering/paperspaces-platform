# **Paper Spaces — AI‑First Contract Commerce OS**

**Democratize world‑class contract lifecycle management (CLM) for 50 million freelancers and lean startups, while embedding a *zero‑friction SaaS commerce layer* inside every agreement.**

Paper Spaces unifies AI drafting, risk review, negotiation, execution, and revenue collection so founders close deals faster *and* software vendors get paid—without lawyers, payment gateways, or tax headaches.

---

## Local Development

### Requirements

To run Paper Spaces locally, you'll need:

- Node.js (v22.0.0 or higher)
- Bun (v1.2.9 or higher)
- Docker (for development database and services)

### Quick Start

1. Clone the repository:
   ```sh
   git clone https://github.com/Vector/paperspaces-platform.git
   cd paperspaces-platform
   ```

2. Install dependencies:
   ```sh
   bun install
   ```

3. Set up your environment variables:
   ```sh
   cp .env.example .env
   ```

4. Start the development environment (includes database setup, migrations, and seeding):
   ```sh
   bun run dx
   ```

5. Start the development server:
   ```sh
   bun run dev
   ```

For the fastest setup experience, use:
```sh
bun run d
```
This command runs the development environment setup and starts the server in one step.

### Additional Commands

- **Translation management:**
  ```sh
  bun run translate
  ```

- **Database commands:**
  ```sh
  bun run prisma:studio    # Open Prisma Studio to manage your database
  bun run prisma:seed      # Seed your database with sample data
  ```

- **Production build:**
  ```sh
  bun run build
  bun run start
  ```


## Reference Architecture (High‑Level)

```
┌────────────────────────────┐              ┌─────────────────────────┐
│  Web / Mobile Frontend     │  GraphQL     │  Third‑Party Integrations│
│  (Next.js, React Native)   │◄────────────►│  • CRM (HubSpot)        │
└──────────────┬─────────────┘              │  • Accounting (QBO)     │
               │REST / gRPC                │  • Cloud Storage (S3)   │
┌──────────────▼─────────────┐              └─────────────────────────┘
│  API Gateway (tRPC)        │
└──────────────┬─────────────┘
               │Event Bus (NATS)
┌──────────────▼─────────────┐   ML Inference  ┌───────────────────────┐
│  Contract Service          │◄───────────────►│  AI Engine (Llama‑3,   │
│  • Clause store            │                │  GPT‑4o via Azure)     │
│  • Template versioning     │                └───────────────────────┘
└──────────────┬─────────────┘
               │ gRPC                        ┌───────────────────────┐
┌──────────────▼─────────────┐               │  Merchant‑of‑Record   │
│  Billing & MoR Service     │──────────────►│  Service (Stripe +    │
│  • Usage metering          │  Webhooks     │  TaxJar, Wise FX)      │
│  • FX & tax calc           │               └───────────────────────┘
└──────────────┬─────────────┘
               │ CDC / Streams              ┌───────────────────────┐
┌──────────────▼─────────────┐               │  Analytics Warehouse  │
│  Event Lake (ClickHouse)   │──────────────►│  (DuckDB/BigQuery)     │
└────────────────────────────┘               └───────────────────────┘
```
