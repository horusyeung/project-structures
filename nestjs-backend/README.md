# NestJS Backend Project Structure

> Production-ready project structure for NestJS with PostgreSQL, Prisma, GraphQL, and MongoDB. Opinionated, scalable, and battle-tested.

## When to Use This

| Use NestJS when... | Consider alternatives when... |
| --- | --- |
| Building enterprise-grade APIs | You need a lightweight serverless function |
| You need both REST and GraphQL | A simple CRUD API with no business logic |
| The project requires strict architectural patterns | Rapid prototyping with minimal structure |
| You need dependency injection and modular architecture | The team is unfamiliar with decorators and DI |
| Multi-database support (PostgreSQL + MongoDB) is required | A single simple database is sufficient |

---

## Project Structure

- The `src/modules/` folder contains feature modules, each with co-located controllers, services, resolvers, DTOs, and tests.
- The `src/common/` folder stores shared guards, interceptors, filters, pipes, decorators, and utilities.
- The `src/config/` folder centralises all configuration (database, auth, app settings).
- The `src/database/` folder contains Prisma schema, migrations, and seed scripts.
- The `src/graphql/` folder contains GraphQL schema and shared types.

```
.
├── .husky/
├── docker/
│   └── docker-compose.yml                       # PostgreSQL + MongoDB + Redis + pgAdmin
│
├── prisma/
│   ├── schema.prisma                            # Prisma schema (PostgreSQL)
│   ├── migrations/                              # Version-controlled migrations
│   └── seed.ts                                  # Database seeding script
│
├── src/
│   ├── modules/                                 # feature modules
│   │   ├── auth/
│   │   │   ├── __tests__/
│   │   │   ├── dto/
│   │   │   │   ├── login.input.ts
│   │   │   │   └── register.input.ts
│   │   │   ├── guards/
│   │   │   │   ├── jwt-auth.guard.ts
│   │   │   │   └── roles.guard.ts
│   │   │   ├── strategies/
│   │   │   │   ├── jwt.strategy.ts
│   │   │   │   └── local.strategy.ts
│   │   │   ├── decorators/
│   │   │   │   ├── current-user.decorator.ts
│   │   │   │   └── roles.decorator.ts
│   │   │   ├── auth.module.ts
│   │   │   ├── auth.service.ts
│   │   │   ├── auth.resolver.ts                 # GraphQL resolver
│   │   │   └── auth.controller.ts               # REST controller (optional)
│   │   │
│   │   ├── users/
│   │   │   ├── __tests__/
│   │   │   ├── dto/
│   │   │   │   ├── create-user.input.ts
│   │   │   │   └── update-user.input.ts
│   │   │   ├── entities/
│   │   │   │   └── user.entity.ts               # GraphQL object type
│   │   │   ├── users.module.ts
│   │   │   ├── users.service.ts
│   │   │   ├── users.resolver.ts
│   │   │   └── users.controller.ts              # REST controller (optional)
│   │   │
│   │   ├── feature1/
│   │   │   ├── __tests__/
│   │   │   ├── dto/
│   │   │   ├── entities/
│   │   │   ├── feature1.module.ts
│   │   │   ├── feature1.service.ts
│   │   │   ├── feature1.resolver.ts
│   │   │   └── feature1.controller.ts
│   │   │
│   │   ├── health/
│   │   │   ├── health.module.ts
│   │   │   └── health.controller.ts             # GET /health
│   │   │
│   │   └── feature2/
│   │       └── ...
│   │
│   ├── common/                                  # shared code
│   │   ├── __mocks__/
│   │   ├── decorators/
│   │   ├── filters/
│   │   │   └── http-exception.filter.ts
│   │   ├── guards/
│   │   ├── interceptors/
│   │   │   ├── logging.interceptor.ts
│   │   │   └── transform.interceptor.ts
│   │   ├── middleware/
│   │   ├── pipes/
│   │   │   └── validation.pipe.ts
│   │   ├── types/
│   │   └── utils/
│   │
│   ├── config/                                  # centralised configuration
│   │   ├── app.config.ts
│   │   ├── database.config.ts
│   │   ├── auth.config.ts
│   │   ├── graphql.config.ts
│   │   └── mongo.config.ts
│   │
│   ├── database/                                # database services
│   │   ├── prisma/
│   │   │   ├── prisma.module.ts
│   │   │   └── prisma.service.ts                # Prisma client wrapper
│   │   └── mongo/
│   │       ├── mongo.module.ts
│   │       └── schemas/                         # Mongoose schemas
│   │           └── activity-log.schema.ts
│   │
│   ├── graphql/                                 # GraphQL config
│   │   ├── graphql.module.ts
│   │   └── scalars/                             # custom scalars
│   │       ├── date.scalar.ts
│   │       └── json.scalar.ts
│   │
│   ├── app.module.ts                            # root module
│   └── main.ts                                  # entry point + Swagger setup
│
├── test/
│   ├── e2e/                                     # end-to-end tests
│   │   └── app.e2e-spec.ts
│   └── jest-e2e.json
│
├── .env.development
├── .env.staging
├── .env.production
├── .env.example
├── .eslintrc.js                                 # or eslint.config.mjs
├── .gitignore
├── .prettierrc
├── docker-compose.yml
├── jest.config.ts
├── nest-cli.json
├── package.json
├── README.md
├── tsconfig.json
├── tsconfig.build.json
└── yarn.lock
```

---

## Environments

| Environment | Description |
| --- | --- |
| Development | Local development with Docker Compose |
| Staging | Pre-production testing and QA |
| Production | Live production |

---

## Tech Stack & Dependencies

### Core

| Category | Tool / Library | Notes |
| --- | --- | --- |
| Framework | [NestJS](https://nestjs.com/) | v10+ with modular architecture |
| Language | [TypeScript](https://www.typescriptlang.org/) | Strict mode enabled |
| Runtime | Node.js 20+ | LTS version |
| Package Manager | [Yarn](https://yarnpkg.com/) | v4 (Berry) |

### Database & ORM

| Category | Tool / Library | Notes |
| --- | --- | --- |
| Primary Database | [PostgreSQL](https://www.postgresql.org/) 16 | Relational data (users, core business entities) |
| ORM | [Prisma](https://www.prisma.io/) | Schema-first, type-safe database access |
| Secondary Database | [MongoDB](https://www.mongodb.com/) | Document store (logs, analytics, unstructured data) |
| ODM | [Mongoose](https://mongoosejs.com/) | MongoDB object modelling via `@nestjs/mongoose` |
| Caching | [Redis](https://redis.io/) (optional) | Session store, caching layer |

### API

| Category | Tool / Library | Notes |
| --- | --- | --- |
| GraphQL | [@nestjs/graphql](https://docs.nestjs.com/graphql/quick-start) + [Apollo Server](https://www.apollographql.com/) | Code-first approach with decorators |
| REST (optional) | Built-in NestJS controllers | For health checks, webhooks, file uploads |
| API Documentation | [Swagger/OpenAPI](https://swagger.io/) via `@nestjs/swagger` | Auto-generated from decorators at `/api/docs` |
| Validation | [class-validator](https://github.com/typestack/class-validator) + [class-transformer](https://github.com/typestack/class-transformer) | DTO validation with decorators |

### Authentication

| Category | Tool / Library | Notes |
| --- | --- | --- |
| Auth | [@nestjs/passport](https://docs.nestjs.com/recipes/passport) + [Passport.js](https://www.passportjs.org/) | Strategy-based authentication |
| JWT | [@nestjs/jwt](https://github.com/nestjs/jwt) | Token-based auth with access/refresh tokens |
| Password Hashing | [bcrypt](https://github.com/kelektiv/node.bcrypt.js) | Industry-standard hashing |

### Testing

| Tool | Purpose |
| --- | --- |
| [Jest](https://jestjs.io/) | Unit and integration testing |
| [Supertest](https://github.com/ladjs/supertest) | HTTP E2E testing |
| [ts-jest](https://kulshekhar.github.io/ts-jest/) | TypeScript support for Jest |

### Linting & Formatting

| Tool | Purpose |
| --- | --- |
| [ESLint](https://eslint.org/) | Linting with TypeScript rules |
| [Prettier](https://prettier.io/) | Code formatting |
| [Husky](https://typicode.github.io/husky/) | Git hooks (pre-commit lint/format) |

### Infrastructure

| Tool | Purpose |
| --- | --- |
| [Docker Compose](https://docs.docker.com/compose/) | Local development services (PostgreSQL, MongoDB, Redis, pgAdmin) |
| [GitHub Actions](https://github.com/features/actions) | CI/CD pipeline |

---

## Database Strategy

### When to Use PostgreSQL (Prisma)

- User accounts, profiles, authentication
- Core business entities with relationships
- Data that requires ACID transactions
- Structured data with strict schemas
- Anything that benefits from relational queries and joins

### When to Use MongoDB (Mongoose)

- Activity logs, audit trails
- Analytics and event tracking
- Unstructured or semi-structured data
- High-write, low-read workloads
- Data that doesn't require relational integrity

### Prisma Schema Conventions

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(uuid())
  email     String   @unique
  name      String?
  password  String
  role      Role     @default(USER)
  createdAt DateTime @default(now()) @map("created_at")
  updatedAt DateTime @updatedAt @map("updated_at")

  @@map("users")
}

enum Role {
  USER
  ADMIN
}
```

**Conventions:**
- Use `uuid()` for primary keys
- Use `@map("snake_case")` for database column names, camelCase for Prisma model fields
- Use `@@map("table_name")` for snake_case table names
- Always include `createdAt` and `updatedAt` timestamps

---

## GraphQL (Code-First Approach)

NestJS with the **code-first** approach generates the GraphQL schema from TypeScript decorators. No `.graphql` files needed.

### Entity (Object Type)

```typescript
// src/modules/users/entities/user.entity.ts
import { ObjectType, Field, ID } from '@nestjs/graphql';

@ObjectType()
export class User {
  @Field(() => ID)
  id: string;

  @Field()
  email: string;

  @Field({ nullable: true })
  name?: string;

  @Field()
  createdAt: Date;
}
```

### Resolver

```typescript
// src/modules/users/users.resolver.ts
import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
import { UseGuards } from '@nestjs/common';
import { User } from './entities/user.entity';
import { UsersService } from './users.service';
import { JwtAuthGuard } from '../auth/guards/jwt-auth.guard';
import { CurrentUser } from '../auth/decorators/current-user.decorator';

@Resolver(() => User)
export class UsersResolver {
  constructor(private readonly usersService: UsersService) {}

  @Query(() => User)
  @UseGuards(JwtAuthGuard)
  me(@CurrentUser() user: User): Promise<User> {
    return this.usersService.findById(user.id);
  }

  @Query(() => [User])
  @UseGuards(JwtAuthGuard)
  users(): Promise<User[]> {
    return this.usersService.findAll();
  }
}
```

### Input DTO

```typescript
// src/modules/users/dto/create-user.input.ts
import { InputType, Field } from '@nestjs/graphql';
import { IsEmail, IsString, MinLength } from 'class-validator';

@InputType()
export class CreateUserInput {
  @Field()
  @IsEmail()
  email: string;

  @Field()
  @IsString()
  @MinLength(8)
  password: string;

  @Field({ nullable: true })
  @IsString()
  name?: string;
}
```

---

## Module Structure

Every feature follows the same modular pattern:

```
src/modules/{feature}/
├── __tests__/
│   ├── feature.service.spec.ts
│   └── feature.resolver.spec.ts
├── dto/
│   ├── create-feature.input.ts
│   └── update-feature.input.ts
├── entities/
│   └── feature.entity.ts          # GraphQL object type
├── feature.module.ts
├── feature.service.ts              # business logic + Prisma queries
├── feature.resolver.ts             # GraphQL resolver
└── feature.controller.ts           # REST controller (optional)
```

### Module Registration

```typescript
// src/modules/feature1/feature1.module.ts
import { Module } from '@nestjs/common';
import { Feature1Service } from './feature1.service';
import { Feature1Resolver } from './feature1.resolver';
import { PrismaModule } from '../../database/prisma/prisma.module';

@Module({
  imports: [PrismaModule],
  providers: [Feature1Service, Feature1Resolver],
  exports: [Feature1Service],
})
export class Feature1Module {}
```

---

## TypeScript Guidelines

- Prefer `type` over `interface` for type definitions. Only use `interface` when declaration merging is explicitly needed (rare).
- Never use the `any` type. Use `unknown` and narrow with type guards when the type is uncertain.
- Use the `satisfies` operator for better type inference with validation.
- Enable `strict` mode and `emitDecoratorMetadata` in `tsconfig.json`.

---

## Configuration

Use `@nestjs/config` with typed configuration:

```typescript
// src/config/database.config.ts
import { registerAs } from '@nestjs/config';

export default registerAs('database', () => ({
  url: process.env.DATABASE_URL,
  mongoUri: process.env.MONGO_URI,
}));
```

### Environment Variables

```
# .env.example
NODE_ENV=development
PORT=3001

# PostgreSQL (Prisma)
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/app_db

# MongoDB
MONGO_URI=mongodb://localhost:27017/app_logs

# Redis (optional)
REDIS_HOST=localhost
REDIS_PORT=6379

# JWT
JWT_SECRET=your-secret-key
JWT_EXPIRES_IN=15m
JWT_REFRESH_EXPIRES_IN=7d
```

---

## Docker Compose

```yaml
# docker/docker-compose.yml
services:
  postgres:
    image: postgres:16-alpine
    container_name: app-postgres
    ports:
      - '5432:5432'
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: app_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U postgres']
      interval: 5s
      timeout: 5s
      retries: 5

  mongo:
    image: mongo:7
    container_name: app-mongo
    ports:
      - '27017:27017'
    volumes:
      - mongo_data:/data/db

  redis:
    image: redis:7-alpine
    container_name: app-redis
    ports:
      - '6379:6379'

  pgadmin:
    image: dpage/pgadmin4:latest
    container_name: app-pgadmin
    ports:
      - '5050:80'
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@app.local
      PGADMIN_DEFAULT_PASSWORD: admin
    depends_on:
      postgres:
        condition: service_healthy
    profiles:
      - tools

volumes:
  postgres_data:
  mongo_data:
```

---

## Testing

### What to test

| Target | Location |
| --- | --- |
| Services | `src/modules/featureName/__tests__/featureName.service.spec.ts` |
| Resolvers | `src/modules/featureName/__tests__/featureName.resolver.spec.ts` |
| Controllers | `src/modules/featureName/__tests__/featureName.controller.spec.ts` |
| Guards | `src/common/guards/__tests__/` |
| Pipes | `src/common/pipes/__tests__/` |
| E2E | `test/e2e/` |

### Testing practices

- Use NestJS `Test.createTestingModule()` for unit tests.
- Mock Prisma client using a custom provider.
- Mock external services — never make real API/database calls in unit tests.
- Use Supertest for E2E tests against the running application.
- Co-locate tests in `__tests__/` folders next to source.

### Example Unit Test

```typescript
// src/modules/users/__tests__/users.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { UsersService } from '../users.service';
import { PrismaService } from '../../../database/prisma/prisma.service';

describe('UsersService', () => {
  let service: UsersService;
  let prisma: PrismaService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        {
          provide: PrismaService,
          useValue: {
            user: {
              findUnique: jest.fn(),
              findMany: jest.fn(),
              create: jest.fn(),
            },
          },
        },
      ],
    }).compile();

    service = module.get<UsersService>(UsersService);
    prisma = module.get<PrismaService>(PrismaService);
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });
});
```

---

## Secrets & Credentials

- Environment-specific config via `.env.*` files (gitignored).
- Never commit secrets to the repository.
- Use `@nestjs/config` with `ConfigService` for type-safe access.
- Store production secrets in a secrets manager (e.g., AWS Secrets Manager, Vault, Doppler).

---

## Common Scripts

```json
{
  "dev": "nest start --watch",
  "build": "nest build",
  "start:prod": "node dist/main",
  "lint": "eslint \"{src,test}/**/*.ts\"",
  "test": "jest",
  "test:watch": "jest --watch",
  "test:cov": "jest --coverage",
  "test:e2e": "jest --config ./test/jest-e2e.json",
  "db:generate": "prisma generate",
  "db:migrate": "prisma migrate dev",
  "db:migrate:deploy": "prisma migrate deploy",
  "db:push": "prisma db push",
  "db:seed": "ts-node prisma/seed.ts",
  "db:studio": "prisma studio",
  "docker:up": "docker compose -f docker/docker-compose.yml up -d",
  "docker:down": "docker compose -f docker/docker-compose.yml down"
}
```

---

## Best Practices Summary

| Rule | Detail |
| --- | --- |
| Unit tests required | Write tests for services, resolvers, and guards |
| Modular architecture | One module per feature with co-located code |
| Prisma for PostgreSQL | All relational data access through Prisma |
| Mongoose for MongoDB | Document storage for logs, analytics, unstructured data |
| GraphQL code-first | Use decorators, not `.graphql` schema files |
| DTOs with validation | Use `class-validator` decorators on all input types |
| No `any` type | Use proper types or `unknown` with type guards |
| `type` over `interface` | Use `interface` only when declaration merging is needed |
| Dependency injection | Never instantiate services directly — use NestJS DI |
| Guards for auth | Use `@UseGuards()` decorator, not manual checks in services |
| Config via `ConfigService` | Never use `process.env` directly in modules |
| Prisma conventions | UUID primary keys, snake_case DB columns, camelCase model fields |
| Health check endpoint | Always expose `GET /health` for monitoring |
| Swagger documentation | Decorate all REST endpoints with `@Api*` decorators |
| Docker for local dev | Use Docker Compose for all infrastructure services |

---

## Key Differences: GraphQL vs. REST in NestJS

| Aspect | GraphQL (Resolver) | REST (Controller) |
| --- | --- | --- |
| Entry point | `@Resolver()` | `@Controller()` |
| Operations | `@Query()`, `@Mutation()`, `@Subscription()` | `@Get()`, `@Post()`, `@Put()`, `@Delete()` |
| Input validation | `@InputType()` + `class-validator` | `@Body()` DTO + `class-validator` |
| Response type | `@ObjectType()` entity | Serialised DTO or entity |
| Documentation | GraphQL Playground / Apollo Studio | Swagger / OpenAPI |
| Use when | Client needs flexible queries, multiple frontends | Webhooks, file uploads, health checks, third-party integrations |

> **Recommendation:** Use GraphQL as the primary API for frontend clients. Keep REST for health checks, webhooks, file uploads, and third-party integrations that expect REST.
