# 🎨 Zora Coins MCP Server

[![npm version](https://badge.fury.io/js/zora-coins-mcp-server.svg)](https://badge.fury.io/js/zora-coins-mcp-server)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A production-ready [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server that provides seamless access to the [Zora Coins](https://zora.co/coins) ecosystem. Query coin data, explore markets, manage profiles, and execute trades on Base mainnet through a simple, standardized interface.

## ✨ Features

### 🔍 **Query Tools** (No wallet required)
- **Market Exploration**: Discover trending coins, top gainers, highest volume, and newest launches
- **Coin Analytics**: Get detailed market data, holder information, and trading history  
- **Social Features**: Access comments, creator profiles, and community engagement
- **Real-time Data**: Live pricing, market caps, and trading volumes

### ⚡ **Write Operations** (Wallet required)
- **Create Coins**: Deploy new creator coins with custom metadata
- **Trade**: Buy/sell coins with ETH or ERC20 tokens
- **Manage**: Update coin metadata and payout recipients

### 🛠️ **Developer Experience**
- **Type Safe**: Built with TypeScript and Zod validation
- **Error Handling**: Comprehensive error messages and graceful failures
- **Pagination**: Support for large datasets with cursor-based pagination
- **Flexible**: Works with any MCP-compatible client (Claude Desktop, Cursor, etc.)

## 🚀 Quick Start

### Installation Options

#### Option 1: NPX (Recommended - No Installation Required)
```bash
# Run directly with npx - always uses latest version
npx zora-coins-mcp-server
```

#### Option 2: Global Installation
```bash
# Install globally for persistent use
npm install -g zora-coins-mcp-server

# Then run with
zora-coins-mcp
```

#### Option 3: Local Installation
```bash
# For project-specific usage
npm install zora-coins-mcp-server

# Then run with
npx zora-coins-mcp-server
```

### Basic Setup

1. **Create environment file:**
```bash
cp .env.example .env
```

2. **Configure environment variables:**
```bash
# Required for enhanced features (get from https://zora.co)
ZORA_API_KEY=your_api_key_here

# Optional: Custom RPC endpoint
BASE_RPC_URL=https://mainnet.base.org

# Required for write operations only
PRIVATE_KEY=0xYourPrivateKeyHere
```

3. **Test the server:**
```bash
zora-coins-mcp
```

## 🔧 MCP Client Integration

### Claude Desktop

Add to your `~/.claude/mcp.json`:

#### Option 1: Using NPX (Recommended)
```json
{
  "mcpServers": {
    "zora-coins": {
      "command": "npx",
      "args": ["zora-coins-mcp-server"],
      "env": {
        "ZORA_API_KEY": "your_api_key_here",
        "BASE_RPC_URL": "https://mainnet.base.org",
        "PRIVATE_KEY": "0xYourPrivateKeyHere"
      }
    }
  }
}
```

#### Option 2: Using Global Installation
```json
{
  "mcpServers": {
    "zora-coins": {
      "command": "zora-coins-mcp",
      "env": {
        "ZORA_API_KEY": "your_api_key_here",
        "BASE_RPC_URL": "https://mainnet.base.org",
        "PRIVATE_KEY": "0xYourPrivateKeyHere"
      }
    }
  }
}
```

### Cursor IDE

Configure in your MCP settings:

#### Option 1: Using NPX (Recommended)
```json
{
  "name": "zora-coins",
  "command": ["npx", "zora-coins-mcp-server"],
  "env": {
    "ZORA_API_KEY": "your_api_key_here"
  }
}
```

#### Option 2: Using Global Installation
```json
{
  "name": "zora-coins",
  "command": ["zora-coins-mcp"],
  "env": {
    "ZORA_API_KEY": "your_api_key_here"
  }
}
```

### Custom Integration

```bash
# Run as stdio server with npx
npx zora-coins-mcp-server

# Or if globally installed
zora-coins-mcp

# Development mode with live reload  
npm run dev
```

## 📖 Available Tools

### 🏥 Health Check
- `zora.health` - Server diagnostics and configuration status

### 🔍 Market Exploration
- `zora.explore_new` - Recently created coins
- `zora.explore_top_gainers` - Biggest 24h gainers
- `zora.explore_top_volume_24h` - Highest trading volume
- `zora.explore_most_valuable` - Highest market cap
- `zora.explore_last_traded` - Recently traded coins

### 💰 Coin Data
- `zora.get_coin` - Comprehensive coin information
- `zora.get_coins` - Batch fetch multiple coins
- `zora.get_coin_holders` - Token holder list with balances
- `zora.get_coin_swaps` - Recent trading activity
- `zora.get_coin_comments` - Community comments

### 👤 Profile Management
- `zora.get_profile` - User profile information
- `zora.get_profile_coins` - Coins created by user
- `zora.get_profile_balances` - User's coin portfolio

### ⚡ Trading & Creation (Requires Wallet)
- `zora.create_coin` - Deploy new creator coin
- `zora.trade_coin` - Buy/sell coins
- `zora.update_coin_uri` - Update metadata
- `zora.update_payout_recipient` - Change earnings recipient

## 💡 Usage Examples

### Explore Trending Coins

```javascript
// Get top 5 newest coins
await mcp.callTool("zora.explore_new", { count: 5 });

// Find biggest gainers
await mcp.callTool("zora.explore_top_gainers", { count: 3 });
```

### Analyze a Specific Coin

```javascript
// Get detailed coin information
await mcp.callTool("zora.get_coin", {
  address: "0xd769d56f479e9e72a77bb1523e866a33098feec5"
});

// Check recent trading activity
await mcp.callTool("zora.get_coin_swaps", {
  address: "0xd769d56f479e9e72a77bb1523e866a33098feec5",
  first: 10
});
```

### Profile Analysis

```javascript
// Get profile information
await mcp.callTool("zora.get_profile", {
  identifier: "base"
});

// See coins created by user
await mcp.callTool("zora.get_profile_coins", {
  identifier: "jacob",
  count: 5
});
```

### Trading Operations

```javascript
// Buy a coin with ETH
await mcp.callTool("zora.trade_coin", {
  sellType: "eth",
  buyType: "erc20", 
  buyAddress: "0x...",
  amount: "0.001",
  slippage: 0.05
});

// Create a new coin
await mcp.callTool("zora.create_coin", {
  name: "My Creator Coin",
  symbol: "MCC",
  uri: "ipfs://...",
  payoutRecipient: "0x..."
});
```

## ⚙️ Configuration

### Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `ZORA_API_KEY` | Recommended | API key from [zora.co](https://zora.co) for enhanced features |
| `BASE_RPC_URL` | Optional | Base mainnet RPC endpoint (defaults to public RPC) |
| `CHAIN_ID` | Optional | Chain ID (defaults to 8453 for Base) |
| `PRIVATE_KEY` | Write ops only | 0x-prefixed private key for transactions |
| `PLATFORM_REFERRER` | Optional | Address for referral attribution |

### Getting API Keys

1. Visit [zora.co](https://zora.co)
2. Go to Developer Settings
3. Generate a new API key
4. Add to your `.env` file

**Note:** The server works without an API key but may have rate limits and reduced functionality.

## 🔒 Security Best Practices

### Private Key Safety
- **Never commit** private keys to version control
- Use environment variables or secure key management
- Consider using a dedicated wallet for trading operations
- Test with small amounts first

### API Key Protection
- Store API keys securely
- Rotate keys regularly
- Monitor usage in Zora dashboard
- Use different keys for development/production

## 🛠️ Development

### Local Development

```bash
# Clone the repository
git clone https://github.com/your-username/zora-coins-mcp-server.git
cd zora-coins-mcp-server

# Install dependencies
npm install

# Copy environment file
cp .env.example .env

# Start development server
npm run dev
```

### Building

```bash
# Build TypeScript
npm run build

# Clean build directory
npm run clean

# Build and start
npm run build && npm start
```

### Project Structure

```
zora-coins-mcp-server/
├── src/
│   └── index.ts          # Main server implementation
├── dist/                 # Compiled JavaScript
├── .env.example          # Environment template
├── package.json          # Package configuration
├── tsconfig.json         # TypeScript configuration
└── README.md            # This file
```

## 🌐 Supported Networks

- **Base Mainnet** (8453) - Full support for all operations
- Other networks may have limited functionality

## 📚 Resources

### Documentation
- [Zora Coins SDK](https://docs.zora.co/coins/sdk) - Official SDK documentation
- [Model Context Protocol](https://modelcontextprotocol.io/) - MCP specification
- [Base Network](https://base.org/) - Layer 2 blockchain documentation

### Community
- [Zora Discord](https://discord.gg/zora) - Community support
- [Base Discord](https://discord.gg/buildonbase) - Network support

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Workflow

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ⚠️ Disclaimer

This software is provided "as is" without warranty. Cryptocurrency trading involves risk. Always:

- Test with small amounts first
- Understand the risks involved
- Never invest more than you can afford to lose
- Do your own research (DYOR)

The Zora Coins on this platform are created for artistic and cultural purposes as collectibles, not as investments or financial instruments.

---

**Built with ❤️ for the Zora ecosystem**

[Report Issues](https://github.com/r4topunk/zora-coins-mcp-server/issues) | [Request Features](https://github.com/r4topunk/zora-coins-mcp-server/issues/new) | [Join Community](https://discord.gg/zora)