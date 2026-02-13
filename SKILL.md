# Dubhe Skill

Dubhe is a high-performance engine for building fully on-chain Move applications on Sui.
This skill provides a complete lifecycle guide: from initialization and contract development to data indexing and frontend integration.

## 🌟 Lifecycle Overview

1.  **Init**: Create project scaffold.
2.  **Develop**: Define Schema in `dubhe.config.ts` (ECS pattern).
3.  **Generate**: Auto-generate Move libraries (`schemagen`).
4.  **Build & Test**: Compile and verify logic.
5.  **Deploy**: Publish to Sui network.
6.  **Index**: Sync on-chain data to SQL via Indexer.
7.  **Serve**: Expose data via GraphQL.
8.  **Consume**: Integrate with frontend apps using Client SDK.

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
This is the heart of your project. Dubhe encourages an **ECS (Entity Component System)** architecture.

**Schema Types:**
-   **StorageValue**: Single value (Global Config).
-   **StorageMap**: Key-Value store (Entity Properties).
-   **StorageDoubleMap**: Double Key Map (Complex Relations).
-   **Events**: Off-chain notifications (no on-chain storage cost).

**Example Config:**
```typescript
import { DubheConfig } from '@0xobelisk/sui-common';

export const dubheConfig = {
  name: 'my_rpg_game',
  description: 'An on-chain RPG',
  schemas: {
    // [Component] Player Level (Map: Address -> u64)
    level: {
      structure: {
        value: 'u64',
      },
    },
    // [Component] Inventory (DoubleMap: Address -> ItemID -> Amount)
    inventory: {
      structure: {
        item_id: 'u64',
        amount: 'u64',
      },
    },
    // [Resource] Global Leaderboard (Value)
    leaderboard: {
      structure: {
        top_players: 'vector<address>',
        scores: 'vector<u64>',
      },
    },
    // [Event] Battle Log (Offchain only)
    battle_log: {
      structure: {
        winner: 'address',
        damage: 'u64',
      },
      is_event: true, // Mark as event
    },
  },
  errors: {
    // Custom Errors
    InsufficientEnergy: { id: 0, message: 'Not enough energy' },
  }
} as DubheConfig;
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
*Note: This deploys both the Schema logic and your custom logic.*

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

**Usage (React/TS):**
```typescript
import { DubheClient } from '@0xobelisk/sui-client';
import { NetworkType } from '@0xobelisk/sui-common';
import { dubheConfig } from './dubhe.config';

// Initialize Client
const client = new DubheClient({
  networkType: NetworkType.TESTNET,
  config: dubheConfig,
  packageId: '0x...', // Your deployed package ID
});

// Query Component (e.g., Player Level)
const userAddress = '0x123...';
const level = await client.getMap('level', userAddress);

// Send Transaction (using wallet adapter)
const tx = await client.tx.level.set(tx, [10]); // Generated helper
```

## 🧰 Troubleshooting & Utilities

-   **`dubhe doctor`**: Diagnose environment issues (Node ver, Sui CLI, Docker).
-   **`dubhe faucet`**: Get testnet SUI tokens.
-   **`dubhe node`**: Start a local Sui node for fast iteration.
-   **`dubhe generate-key`**: Create a new keypair for deployment.

## 📂 Project Structure

```
my-dubhe-app/
├── dubhe.config.ts        # 1. Config Source (Schema Definitions)
├── build/                 # 2. Compilation Artifacts
├── src/                   # 3. Frontend Code (if web template)
├── sources/               # 4. Move Contracts (Auto-generated + Custom)
├── tests/                 # 5. Move Tests
└── package.json           # 6. Scripts
```
