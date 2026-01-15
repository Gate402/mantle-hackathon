# Gate402 - Web3 API Payment Gateway

**Built for Mantle X402 Hackathon**

Gate402 is a Web3-native payment gateway that enables developers to monetize their APIs with instant, permissionless cryptocurrency payments using the x402 protocol. Create a gateway, set your price, and get paid automatically for every API call—no traditional payment infrastructure required.

## 🚀 What is Gate402?

Gate402 transforms any HTTP API into a monetizable service through blockchain-powered micropayments. Whether you're serving AI models, weather data, market analytics, or custom tools, Gate402 provides a simple gateway that sits in front of your API and handles payments automatically.

### How It Works

```
Client → Gate402 Gateway → Payment Verification (x402) → Your API → Response
```

1. **You have an API** - Any HTTP endpoint you want to monetize
2. **Create a Gateway** - Configure your gateway with a unique subdomain (e.g., `alice-weather.gate402.xyz`)
3. **Set a Price** - Choose how much to charge per request (e.g., $0.01 per call)
4. **Share the URL** - AI agents and developers use your gateway URL; payments happen automatically via x402 headers
5. **Get Paid** - Receive cryptocurrency payments directly to your wallet

### Key Features

- **🔐 Permissionless Monetization** - No payment processors, bank accounts, or complex integrations
- **⚡ Instant Settlements** - Payments settled on-chain in real-time using x402 protocol
- **💰 Micropayments** - Support for payments as low as $0.001 per request
- **🛡️ Gateway Protection** - Your API is only accessible after payment verification
- **📊 Analytics Dashboard** - Track requests, revenue, and top customers
- **🎯 Flexible Pricing** - Set custom prices per API call
- **🔗 Multi-Chain Support** - Solana USDC, Base USDC (expandable)
- **🎨 Beautiful UI/UX** - Modern, responsive interface with dark mode

## 📦 Repository Structure

This monorepo contains three main components:

```
mantle-hackathon/
├── core-engine/        # Backend API & payment proxy
├── smart-contract/     # GAToken (G402) ERC20 contracts
└── ui/                 # Frontend dashboard
```

### Components

#### 1. **Core Engine** (Backend)

Express.js backend that handles:

- Gateway management API
- x402 payment verification
- HTTP proxying to origin servers
- Analytics and logging
- Multi-chain payment support (Solana, Base)

**Tech Stack:** Express.js, TypeScript, Prisma, PostgreSQL, Redis

[📖 Core Engine Documentation](https://github.com/Gate402/core-engine/README.md)

#### 2. **Smart Contract**

GAToken (G402) - ERC20 token with:

- EIP-3009 (gasless transfers via meta-transactions)
- ERC20Permit (gasless approvals)
- ERC20Burnable
- Role-based minting

**Tech Stack:** Solidity, Foundry

[📖 Smart Contract Documentation](https://github.com/Gate402/gate-token-sc/README.md)

#### 3. **UI** (Frontend)

React-based dashboard for:

- Gateway creation and management
- Analytics visualization
- Wallet integration
- Payment monitoring

**Tech Stack:** React 19, TypeScript, Vite, Wagmi, RainbowKit, Tailwind CSS

[📖 UI Documentation](https://github.com/Gate402/mantle-hackathon-ui/README.md)

## 🛠️ Quick Start

### Prerequisites

- Node.js 18+
- PostgreSQL 14+
- Redis 6+
- Foundry (for smart contracts)
- A Web3 wallet (MetaMask, WalletConnect, etc.)

### Installation

```bash
# Clone the repository
git clone https://github.com/gate402/mantle-hackathon.git
cd mantle-hackathon
```

### Setup & Configuration

Each component has its own detailed setup instructions:

- **[Core Engine Setup](https://github.com/Gate402/core-engine/README.md)** - Backend API, payment proxy, database configuration
- **[UI Setup](https://github.com/Gate402/mantle-hackathon-ui/README.md)** - Frontend dashboard, environment configuration, development server
- **[Smart Contract Setup](https://github.com/Gate402/gate-token-sc/README.md)** - Contract deployment, Foundry configuration

Please refer to the individual component documentation for detailed installation, configuration, and deployment instructions.

## 🎯 Usage Guide

### For API Providers

1. **Connect Wallet** - Connect your Web3 wallet via RainbowKit
2. **Create Gateway**:
   - Click "Create Gateway" in the dashboard
   - Enter your API endpoint URL (e.g., `https://my-api.example.com`)
   - Set price per request (e.g., $0.01)
   - Choose accepted payment networks
3. **Get Gateway URL** - Receive your unique subdomain (e.g., `alice-weather.gate402.xyz`)
4. **Configure Origin** - Add the `X-Gate402-Secret` header verification to your API
5. **Share & Earn** - Share your gateway URL with developers and AI agents

### For API Consumers

1. **Connect Wallet** - Connect your Web3 wallet
2. **Make Request** - Send HTTP request to gateway URL with `x402-payment` header
3. **Automatic Payment** - Payment is verified via blockchain
4. **Receive Response** - Get API response after payment confirmation

### Example Request

```bash
curl -X GET "https://alice-weather.gate402.xyz/weather?city=SF" \
  -H "x402-payment: <transaction_hash>"
```

## 🏗️ Architecture

### Request Flow

```
1. Request arrives → alice-weather.gate402.xyz
2. Extract subdomain → Look up gateway config (cached in Redis)
3. Check x402-payment header
   - Missing → Return 402 Payment Required
   - Present → Validate payment on blockchain
4. Payment validation
   - Invalid → Return 402 Payment Required
   - Valid → Proxy to origin server
5. Add X-Gate402-Secret header
6. Forward request to origin
7. Log analytics (request, payment, performance)
8. Return response to client
```

### Database Schema

- **User** - Gateway owners with payout wallets
- **Gateway** - Subdomain configs, pricing, origin URLs
- **Payment** - Blockchain transaction records
- **RequestLog** - Analytics and payment funnel tracking

### Caching Strategy

- **Gateway configs** - 5 minutes (Redis)
- **Payment validations** - 1 hour (Redis)
- **Analytics queries** - Optimized with PostgreSQL indexes

## 📊 Tech Stack Summary

| Component            | Technologies                                      |
| -------------------- | ------------------------------------------------- |
| **Backend**          | Express.js, TypeScript, Prisma, PostgreSQL, Redis |
| **Frontend**         | React 19, Vite, Wagmi, RainbowKit, Tailwind CSS   |
| **Smart Contracts**  | Solidity, Foundry, OpenZeppelin                   |
| **Blockchain**       | Mantle                                            |
| **Payment Protocol** | x402                                              |
| **DevOps**           | Docker                                            |

## 🚀 Deployment

### Backend

1. Set up PostgreSQL and Redis instances
2. Configure environment variables in your deployment platform
3. Deploy from Git repository
4. Verify health check at `/health`
5. Configure wildcard DNS: `*.gate402.xyz` → your server

### Frontend (Vercel/Netlify)

```bash
cd ui
npm run build
# Deploy dist/ directory
```

### Smart Contracts

```bash
cd smart-contract
forge script script/DeployGAToken.s.sol:DeployGAToken \
  --rpc-url <rpc_url> \
  --broadcast \
  --verify
```

## 📈 Analytics & Monitoring

The platform tracks:

- **Revenue Metrics** - Total, 24h, 7d, 30d
- **Request Analytics** - Total requests, success rate, conversion funnel
- **Top Customers** - Wallets ranked by spend
- **Payment Breakdown** - By network, time period
- **Gateway Performance** - Response times, error rates

## 🔒 Security

- **Payment Validation** - Blockchain verification before proxying
- **Secret Tokens** - Cryptographically secure gateway tokens
- **Rate Limiting** - API endpoint protection
- **Input Sanitization** - URL and subdomain validation
- **Origin Protection** - URLs never exposed to clients
- **Proxy Isolation** - Each gateway proxies independently

## 🤝 Contributing

We welcome contributions! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License.

## 🔗 Links

- **Live Demo**: [Coming Soon]
- **Documentation**: [Link to docs]
- **Twitter**: [@Gate402]

## 👥 Team

Built with ❤️ by the Gate402 team for the Mantle X402 Hackathon.

---

## 🎮 Hackathon Submission

**Category**: DApp Development

**Key Innovations**:

- First payment gateway built on x402 protocol
- Seamless API monetization without traditional infrastructure
- Multi-chain payment support with instant settlements
- Developer-friendly integration with subdomain-based routing
- Real-time analytics for gateway performance

**Technical Highlights**:

- Custom ERC20 token (G402) with EIP-3009 gasless transfers
- Redis-cached payment validation for sub-millisecond response times
- PostgreSQL-backed analytics with optimized indexes
- Beautiful React UI with smooth animations and dark mode
- Comprehensive TypeScript implementation across all components

**Impact**: Enabling the next generation of AI agents and automated services to pay for API access programmatically, without human intervention.

---
