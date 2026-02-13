# Dubhe Skill for OpenClaw

**Dubhe Skill** empowers AI agents (running on OpenClaw) to build, deploy, and manage fully on-chain applications using the [Dubhe Engine](https://github.com/0xobelisk/dubhe).

This skill encapsulates the entire development lifecycle of Dubhe projects, enabling AI to act as an expert Move developer.

## 🌟 Capabilities

With this skill, your AI agent can:

- **🚀 Initialize Projects**: Scaffold new Dubhe dApps with `pnpm create dubhe`.
- **⚙️ Manage Schema (ECS)**: Configure complex data models (`StorageValue`, `StorageMap`, `StorageDoubleMap`) in `dubhe.config.ts`.
- **🛠️ Build & Test**: Automatically compile contracts and run Move unit tests.
- **📦 Deploy**: Publish contracts to Sui Testnet/Mainnet.
- **📊 Index Data**: Set up and run the Dubhe Indexer to sync on-chain data to PostgreSQL.
- **🔌 Serve API**: Launch a GraphQL server for frontend consumption.
- **🖥️ Generate Client**: Create TypeScript SDKs for frontend integration.

## 📦 Installation

To use this skill in your OpenClaw instance:

1.  **Clone the repository** into your skills directory:
    ```bash
    cd /home/admin/.openclaw/workspace/skills
    git clone https://github.com/0xagentlabs/dubhe-skill.git dubhe
    ```

2.  **Verify** installation:
    Ask your agent: *"Check if the dubhe skill is available."*

## 🤖 Usage Examples

Once installed, you can ask your AI agent to perform complex tasks:

### Initialize a Project
> "Create a new Dubhe project named 'my-game' using the 101 template."

### Define Data Schema
> "Update dubhe.config.ts to add a 'Player' component with level (u64) and inventory (vector<u64>)."

### Deploy to Testnet
> "Build the contracts and publish them to Sui Testnet. Make sure to use the faucet first."

### Start Indexer
> "Convert the config to JSON and start the Dubhe Indexer connected to my local Postgres."

## 📚 Documentation

The core logic and detailed instructions for the AI are located in [SKILL.md](./SKILL.md).
This file serves as the "brain" for the agent, containing:
- CLI command references (`dubhe-cli`)
- Configuration patterns
- Troubleshooting steps

## 🔗 Resources

- [Dubhe Official Docs](https://dubhe-docs.obelisk.build/)
- [Dubhe GitHub](https://github.com/0xobelisk/dubhe)
- [Sui Network](https://sui.io/)

## 📄 License

MIT
