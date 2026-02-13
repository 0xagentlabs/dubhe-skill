# Dubhe Skill

Dubhe is a high-performance engine for building fully on-chain Move applications (primarily on Sui).
This skill provides instructions for initializing, configuring, building, and deploying Dubhe projects.

## Usage

### 1. Initialization (New Project)
To create a new Dubhe project, use `pnpm create dubhe`. This is an interactive command, so you must use the `process` tool or `exec` with `pty=true` if automation is needed, but typically you should run it and let the user interact or handle the prompts if you know them.

```bash
# Start the interactive creator
pnpm create dubhe
# Follow prompts: Project Name -> Template (101, web, contract)
```

### 2. Standard Workflow
Once inside a Dubhe project (where `dubhe.config.ts` exists), use the local CLI.
**Note:** The CLI is installed as a dev dependency. Use `pnpm exec dubhe` or the scripts defined in `package.json`.

#### Common Commands
Run these from the project root:

- **Check Environment:**
  ```bash
  pnpm exec dubhe doctor
  ```

- **Generate Schemas:**
  Regenerate Store libraries from `dubhe.config.ts`.
  ```bash
  pnpm exec dubhe schemagen
  ```

- **Build Contracts:**
  ```bash
  pnpm exec dubhe build
  ```

- **Run Tests:**
  ```bash
  pnpm exec dubhe test
  ```

- **Deploy (Publish):**
  Deploys contracts to the configured network (default: testnet/localnet).
  Requires `PRIVATE_KEY` in `.env`.
  ```bash
  pnpm exec dubhe publish
  ```

- **Start Local Node:**
  ```bash
  pnpm exec dubhe node
  ```

- **Request Faucet:**
  ```bash
  pnpm exec dubhe faucet
  ```

### 3. Configuration (`dubhe.config.ts`)
The `dubhe.config.ts` file is the heart of the project. It defines schemas, name, and network settings.
Example structure:
```typescript
import { DubheConfig } from '@0xobelisk/sui-common';

export const dubheConfig = {
  name: 'my_project',
  description: 'My Dubhe Project',
  schemas: {
    // Define your schemas here
  }
} as DubheConfig;
```

### 4. Environment Variables
Ensure a `.env` file exists for deployment:
```
PRIVATE_KEY=0x...
```
You can generate one with:
```bash
pnpm exec dubhe generate-key
```

## Tips
- Always run `dubhe doctor` if you encounter environment issues.
- `dubhe schemagen` must be run after modifying `dubhe.config.ts`.
- Use `pnpm exec dubhe --help` to see all available commands.
