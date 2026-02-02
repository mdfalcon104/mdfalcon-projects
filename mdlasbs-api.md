# mdlasbs-api

**Monorepo serverless API platform for crypto data (balances, prices, bridge bot) built with AWS CDK, Lambda, and Lerna.**

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | TypeScript |
| Monorepo | Lerna 8 + npm workspaces |
| Infrastructure | AWS CDK (CloudFormation) |
| Compute | AWS Lambda (Node.js 18.x) |
| API | Amazon API Gateway (REST, custom domain, TLS 1.2) |
| Database | Amazon DynamoDB (on-demand billing) |
| Messaging | Amazon SNS (topics) + SQS (queues with DLQ) |
| DNS / TLS | Route 53, ACM (SSL certificates) |
| Monitoring | CloudWatch Dashboard |
| Local Dev | Express.js for local Lambda emulation |

## Architecture Overview

```
mdlabs-api-v3/
├── package.json                 # Root workspace config, Lerna scripts
├── lerna.json                   # Independent versioning, npm client
├── tsconfig.json                # Base TypeScript config
├── apps/
│   ├── balance-module/          # Lambda: user balance lookups (DynamoDB)
│   │   └── src/handlers/
│   │       ├── balance.ts       # GET /balance -- query by userId + currency
│   │       ├── queue.ts         # SQS consumer for balance updates
│   │       └── index.ts         # Handler exports
│   ├── price-module/            # Lambda: crypto price lookups (DynamoDB)
│   │   └── src/handlers/
│   │       ├── price.ts         # GET /price -- query by symbol + currency
│   │       └── queue.ts         # SQS consumer for price updates
│   └── bridge_zero_bot_module/  # Lambda: bridge/bot operations
│       └── src/handlers/
│           ├── bridge_zero.ts   # Bot handler logic
│           ├── queue.ts         # SQS consumer for bot events
│           └── index.ts         # Handler exports
├── libs/
│   ├── common/                  # Shared utilities, environment config
│   └── types/                   # Shared TypeScript type definitions
└── infrastructure/
    ├── lib/
    │   └── base-stack.ts        # Full CDK stack definition
    ├── bin/                     # CDK app entry point
    └── cdk.json                 # CDK configuration
```

## Key Features

- **Multi-Module Monorepo** -- three independent Lambda modules managed by Lerna with shared libraries.
- **Balance API** -- query user balances by userId and currency from DynamoDB.
- **Price API** -- query crypto prices by symbol and currency from DynamoDB.
- **Bridge Zero Bot** -- automated bot module for bridge operations with SQS event processing.
- **Event-Driven Architecture** -- SNS topics fan out to SQS queues; each module has its own queue consumer with dead-letter queues (3 retries, 14-day retention).
- **Infrastructure as Code** -- entire stack (API Gateway, Lambda, DynamoDB, SNS, SQS, Route 53, ACM, CloudWatch) defined in a single CDK stack with environment-based naming.
- **Multi-Environment** -- `deploy:dev` and `deploy:prod` scripts; domain names, table names, and resources are environment-scoped.
- **CloudWatch Dashboard** -- monitors API request counts and queue message visibility.

## How It Works

1. **Build**: `lerna run build` compiles all TypeScript modules in `apps/` to `dist/`.
2. **Deploy**: CDK synthesizes a CloudFormation template from `base-stack.ts`, provisioning:
   - DynamoDB tables (`balance-{env}`, `price-{env}`) with on-demand billing
   - SNS topics and SQS queues (with DLQ) for async event processing
   - Lambda functions for each module, wired to API Gateway routes and SQS triggers
   - API Gateway with custom domain, TLS 1.2, and CORS
   - Route 53 alias record pointing the domain to the API Gateway
3. **Runtime**: HTTP requests hit API Gateway, which invokes the corresponding Lambda handler. Handlers query DynamoDB and return JSON responses with CORS headers. Async events flow through SNS to SQS queues, processed by dedicated queue handler Lambdas.
4. **Local Dev**: Each module includes a `local-dev.ts` that spins up an Express server emulating Lambda locally.
