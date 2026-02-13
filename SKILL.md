# Dubhe Skill

Dubhe is a high-performance engine for building fully on-chain Move applications (primarily on Sui).
This skill provides comprehensive instructions for initializing, configuring, building, testing, and deploying Dubhe projects.

## Usage

### 1. Initialization (New Project)
To create a new Dubhe project, use `pnpm create dubhe`. This is an interactive command.
If automating, use `process` tool or `exec` with `pty=true` to handle prompts.

```bash
# Start the interactive creator
pnpm create dubhe
# Follow prompts: Project Name -> Template (101, web, contract)
```

**Templates:**
- `101`: A basic starter project.
- `web`: Includes frontend integration examples.
- `contract`: Focuses purely on Move contracts.

### 2. Project Structure
A standard Dubhe project layout:

```
my-dubhe-project/
├── dubhe.config.ts       # Core configuration (Schemas, Network, etc.)
├── Move.toml             # Move package manifest
├── sources/              # Your Move smart contracts (.move files)
├── tests/                # Move unit tests
├── .env                  # Environment variables (PRIVATE_KEY)
└── package.json          # Scripts and dependencies
```

### 3. Core Workflow
Run these commands from the project root. The CLI is a local dev dependency.

#### Development Cycle
1.  **Doctor Check**: Ensure your environment (Sui CLI, Node, etc.) is ready.
    ```bash
    pnpm exec dubhe doctor
    ```
2.  **Generate Keys**: Create a deployer account if you don't have one.
    ```bash
    pnpm exec dubhe generate-key
    ```
    *Check `.env` for the generated private key and address.*

3.  **Build Contracts**: Compile your Move code.
    ```bash
    pnpm exec dubhe build
    ```

4.  **Test**: Run unit tests.
    ```bash
    pnpm exec dubhe test
    ```

5.  **Deploy (Publish)**: Deploy to the network.
    ```bash
    pnpm exec dubhe publish
    ```
    *Default network is usually `testnet` or `localnet` depending on config.*

#### Schema Management
When you modify `dubhe.config.ts`, you MUST regenerate the Store libraries.
```bash
pnpm exec dubhe schemagen
```
This updates the Move code that handles your data structures.

#### Network & Faucet
- **Start Local Node**:
  ```bash
  pnpm exec dubhe node
  ```
- **Request Tokens**:
  ```bash
  pnpm exec dubhe faucet
  ```
- **Switch Environment**:
  ```bash
  pnpm exec dubhe switch-env <network>
  ```

### 4. Configuration (`dubhe.config.ts`)
This file defines your data schemas and project settings.

**Example:**
```typescript
import { DubheConfig } from '@0xobelisk/sui-common';

export const dubheConfig = {
  name: 'my_game',
  description: 'My On-Chain Game',
  schemas: {
    // Define a schema for a player
    player: {
      name: 'String',
      level: 'u64',
      inventory: 'vector<u64>',
    },
    // Define a schema for game state
    gameState: {
      score: 'u64',
      isActive: 'bool',
    }
  }
} as DubheConfig;
```
*After changing this file, run `pnpm exec dubhe schemagen`.*

### 5. Troubleshooting
- **Build Errors?** Ensure `Move.toml` dependencies are up to date and valid.
- **Deploy Fails?** Check `.env` for `PRIVATE_KEY` and ensure the account has gas (use `faucet`).
- **Schema Mismatch?** If your Move code complains about missing structs, run `schemagen`.
- **Environment Issues?** `dubhe doctor` is your best friend.

## Tips
- The CLI is installed locally. Always prefix with `pnpm exec` or define scripts in `package.json`.
- Keep your `dubhe.config.ts` clean; it's the source of truth for your data models.
