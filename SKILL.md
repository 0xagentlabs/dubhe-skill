# Dubhe Skill

Dubhe is a high-performance engine for building fully on-chain Move applications on Sui.
This skill provides a complete lifecycle guide: from initialization and contract development to data indexing and frontend integration.

## 🌟 Lifecycle Overview

1.  **Init**: Create project scaffold.
2.  **Develop**: Write Move contracts, define schemas in `dubhe.config.ts`.
3.  **Build & Test**: Compile and verify logic.
4.  **Deploy**: Publish to Sui network.
5.  **Index**: Sync on-chain data to SQL via Indexer.
6.  **Serve**: Expose data via GraphQL.
7.  **Consume**: Integrate with frontend apps using Client SDK.

## 🛠️ Usage Guide

### Phase 1: Initialization

Create a new project using the interactive generator.
```bash
pnpm create dubhe
# Select template: '101' (basic), 'web' (fullstack), or 'contract'
```

### Phase 2: Contract Development (The Loop)

Run these from project root using `pnpm exec dubhe <cmd>`.

#### 1. Configuration (`dubhe.config.ts`)
Define your data models here. This is the source of truth.
```typescript
export const dubheConfig = {
  name: 'my_dapp',
  schemas: {
    hero: { level: 'u64', exp: 'u64' } // Automatically generates Move structs & TS types
  }
}
```

#### 2. Code Generation
**Crucial Step:** Whenever `dubhe.config.ts` changes, run this to update Move and TS bindings.
```bash
pnpm exec dubhe schemagen
```

#### 3. Build & Test
```bash
pnpm exec dubhe build
pnpm exec dubhe test
```

#### 4. Deploy (Publish)
Ensure `.env` has `PRIVATE_KEY` with gas.
```bash
pnpm exec dubhe publish
```

#### 5. Upgrade
To upgrade deployed packages (requires UpgradeCap):
```bash
pnpm exec dubhe upgrade
```

### Phase 3: Data Indexing (Indexer)

Dubhe Indexer syncs on-chain events/objects to a PostgreSQL database.

**Prerequisites:**
- PostgreSQL running (e.g., via Docker).
- `DATABASE_URL` set in `.env`.

**Steps:**
1.  **Convert Config:** Indexer needs JSON format.
    ```bash
    pnpm exec dubhe convert-json --config-path dubhe.config.ts
    ```
2.  **Start Indexer:**
    ```bash
    # Standard start
    dubhe-indexer --config dubhe.config.json --network testnet --with-graphql

    # OR via Docker (production)
    docker run -e DATABASE_URL=... 0xobelisk/dubhe-indexer ...
    ```

### Phase 4: GraphQL API

Expose the indexed data via a GraphQL endpoint.

```bash
# Start server (default port 4000)
dubhe-graphql-server start
```
*Connects to the same PostgreSQL database as the Indexer.*

### Phase 5: Frontend Integration

Use the generated TS client to interact with your dApp.

**Installation:**
```bash
pnpm add @0xobelisk/sui-client @0xobelisk/sui-common
```

**Usage:**
```typescript
import { DubheClient } from '@0xobelisk/sui-client';
import { NetworkType } from '@0xobelisk/sui-common';
import { dubheConfig } from './dubhe.config';

const client = new DubheClient({
  networkType: NetworkType.TESTNET,
  config: dubheConfig,
  // ... metadata from deploy
});

// Query data
const hero = await client.getObject('hero', objectId);
```

## 🧰 Troubleshooting & Utilities

-   **`dubhe doctor`**: Diagnose environment issues (Node ver, Sui CLI, Docker).
-   **`dubhe faucet`**: Get testnet SUI tokens.
-   **`dubhe node`**: Start a local Sui node for fast iteration.
-   **`dubhe generate-key`**: Create a new keypair for deployment.

## 📂 Project Structure

```
my-dubhe-app/
├── dubhe.config.ts        # 1. Config Source
├── build/                 # 2. Compilation Artifacts
├── src/                   # 3. Frontend Code (if web template)
├── sources/               # 4. Move Contracts
├── tests/                 # 5. Move Tests
└── package.json           # 6. Scripts
```
