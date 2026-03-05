# 🌍 SomoHuru Web3 LMS

> **AI-Powered Inclusive Learning Platform for K-12 Learners in Africa**  
> Built on WordPress + Polygon Blockchain | Open Source | Digital Public Good

[![License: GPL-2.0](https://img.shields.io/badge/License-GPL%202.0-blue.svg)](https://opensource.org/licenses/GPL-2.0)
[![Blockchain: Polygon](https://img.shields.io/badge/Blockchain-Polygon%20PoS-8247E5)](https://polygon.technology)
[![Status: Prototype](https://img.shields.io/badge/Status-Prototype-orange)]()
[![UNICEF Venture Fund](https://img.shields.io/badge/Applying-UNICEF%20Venture%20Fund-00AEEF)]()

---

## 📖 What is SomoHuru?

**SomoHuru** (*"Free Learning"* in Swahili) is an AI-powered, inclusive K-12 learning platform designed for underserved communities across Africa. It delivers personalised, accessible education while using blockchain technology to give every student a permanent, self-sovereign record of their achievements.

In fragile contexts where schools close, governments change, and paper records vanish — SomoHuru ensures a child's educational progress is immutably recorded, student-owned, and globally verifiable.

### The Problem We Solve

- 📉 **244 million** children across sub-Saharan Africa remain out of school or without quality learning
- 📄 Educational records are routinely **lost, falsified, or erased** when institutions collapse
- 🚫 Existing platforms **lack inclusive design** for learners with disabilities
- 💸 No meaningful **incentive system** exists to keep vulnerable students engaged in learning

### Our Solution: Learn & Earn

| Feature | Description |
|---|---|
| 🤖 AI Personalisation | Adaptive lessons tailored to each learner's pace and ability |
| ♿ Inclusive Design | Accessible content for visual, hearing, and cognitive disabilities |
| 🏆 NFT Certificates | Tamper-proof, student-owned credentials minted on Polygon |
| 🪙 SOMO Tokens | ERC-20 rewards earned for completing lessons and quizzes |
| 🗳️ DAO Governance | Community-driven decision-making for students and teachers |
| 📦 IPFS Storage | Decentralised, censorship-resistant certificate metadata |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                   FRONT-END (Web2)                  │
│         WordPress LMS + Custom Plugin               │
│   Courses · Lessons · Quizzes · Teacher Dashboard   │
└─────────────────┬───────────────────────────────────┘
                  │ Web3.js / Ethers.js
┌─────────────────▼───────────────────────────────────┐
│                  WEB3 BRIDGE                        │
│         MetaMask · Wallet Connect                   │
│    Triggers smart contract calls on completion      │
└─────────────────┬───────────────────────────────────┘
                  │ Solidity Smart Contracts
┌─────────────────▼───────────────────────────────────┐
│             POLYGON PoS (Layer 2)                   │
│  ERC-721 NFT Certs · ERC-20 SOMO Token · DAO        │
│     Public · Permissionless · ~$0.001/tx            │
└─────────────────┬───────────────────────────────────┘
                  │ IPFS URI stored on-chain
┌─────────────────▼───────────────────────────────────┐
│           DECENTRALISED STORAGE (IPFS)              │
│     Certificate metadata · Pinata · nft.storage     │
└─────────────────────────────────────────────────────┘
```

---

## 🔧 Tech Stack

| Layer | Technology |
|---|---|
| Front-End | WordPress, PHP, JavaScript, Web3.js |
| Blockchain | Polygon PoS (Layer 2, Ethereum-compatible) |
| Smart Contracts | Solidity, OpenZeppelin ERC-721 / ERC-20 / Governor |
| Wallet | MetaMask, WalletConnect |
| Storage | IPFS, Pinata |
| AI / LMS | WordPress LMS, Custom AI Personalisation Layer |
| Dev Tools | Remix IDE, Hardhat, OpenZeppelin |

---

## 📦 Repository Structure

```
somohuru-web3-lms/
├── somohuru-web3-lms.php       # Main WordPress plugin file
├── includes/
│   ├── shortcodes.php          # All frontend shortcodes
│   ├── ajax.php                # AJAX handlers (lesson, quiz, DAO, wallet)
│   ├── admin.php               # WordPress admin dashboard
│   └── api.php                 # REST API endpoints
├── assets/
│   ├── css/somo.css            # Full stylesheet
│   └── js/somo.js              # Web3 + LMS JavaScript
├── contracts/                  # Solidity smart contracts (coming soon)
│   ├── SomoHuruCertificate.sol # ERC-721 NFT certificate contract
│   ├── SOMOToken.sol           # ERC-20 reward token contract
│   └── SomoHuruDAO.sol         # DAO governance contract
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- WordPress 6.0+
- PHP 7.4+
- MetaMask browser extension
- Node.js 16+ (for contract deployment)

### WordPress Plugin Installation

1. Download the latest release zip from the [Releases](../../releases) page
2. In WordPress Admin → **Plugins → Add New → Upload Plugin**
3. Upload `somohuru-web3-lms.zip` and click **Activate**
4. A demo course and all database tables are created automatically

### Configuration

Go to **WordPress Admin → SomoHuru → Web3 Settings** and enter:

```
Polygon RPC URL:        https://rpc-mumbai.maticvigil.com (testnet)
                        https://polygon-rpc.com (mainnet)
NFT Contract Address:   0x... (after deploying SomoHuruCertificate.sol)
Token Contract Address: 0x... (after deploying SOMOToken.sol)
DAO Contract Address:   0x... (after deploying SomoHuruDAO.sol)
Pinata API Key:         [from pinata.cloud]
Pinata Secret:          [from pinata.cloud]
```

### Shortcodes

Place these on any WordPress page:

| Shortcode | Description |
|---|---|
| `[somo_dashboard]` | Full student portal — courses, DAO, NFTs, wallet |
| `[somo_course]` | Lesson viewer with quiz and completion tracking |
| `[somo_wallet]` | MetaMask connect / disconnect widget |
| `[somo_dao]` | DAO governance — proposals and voting |
| `[somo_nft_gallery]` | Student's earned NFT certificates |

---

## ⛓️ Smart Contracts

> ⚠️ Contracts are currently in development and have **not yet been audited**. Do not use on mainnet with real funds until a formal audit is complete.

### NFT Certificate (ERC-721)

```solidity
// SomoHuruCertificate.sol
// Mints a unique NFT certificate when a student completes a course
// Metadata stored on IPFS via Pinata

function mintCertificate(address student, string memory metadataURI) public onlyOwner {
    uint256 tokenId = tokenCounter;
    _safeMint(student, tokenId);
    tokenURIs[tokenId] = metadataURI;
    tokenCounter++;
}
```

### SOMO Token (ERC-20)

```solidity
// SOMOToken.sol
// Rewards students for learning milestones

function rewardStudent(address student, uint256 amount) public onlyOwner {
    _mint(student, amount * 10 ** decimals());
}
```

### Deploying to Polygon Mumbai Testnet

```bash
# Install dependencies
npm install --save-dev hardhat @openzeppelin/contracts

# Deploy
npx hardhat run scripts/deploy.js --network mumbai
```

Get free test MATIC from the [Polygon Mumbai Faucet](https://faucet.polygon.technology).

---

## 🗳️ DAO Governance

SomoHuru's DAO gives students and teachers real ownership over platform decisions:

- 📝 **Submit proposals** (requires 10 SOMO tokens)
- 🗳️ **Vote** on curriculum changes, reward rates, partnerships
- 🏛️ **On-chain governance** via OpenZeppelin Governor
- 📊 **Off-chain voting** via [Snapshot.org](https://snapshot.org) (gasless)

---

## 🗺️ Roadmap

| Phase | Milestone | Status |
|---|---|---|
| ✅ Phase 1 | WordPress LMS built, smart contracts written | **Complete** |
| 🔄 Phase 2 | Testnet deployment, MetaMask integration, IPFS pipeline | **In Progress** |
| ⏳ Phase 3 | Smart contract audit, mainnet deployment | **Planned** |
| ⏳ Phase 4 | Pilot launch — 50–100 students, 2 partner schools in Kenya | **Planned** |
| ⏳ Phase 5 | SOMO token launch, DAO governance activation | **Planned** |
| ⏳ Phase 6 | Regional expansion — East Africa | **Planned** |

---

## 🌱 Impact Metrics (Target — Pilot Phase)

| Metric | Target |
|---|---|
| Students onboarded | 50–100 |
| Courses completed | 200+ |
| NFT certificates minted | 100+ |
| SOMO tokens distributed | 10,000+ |
| DAO proposals submitted | 5+ |
| Partner schools | 2 |

---

## 🤝 Contributing

SomoHuru is an open-source Digital Public Good. Contributions are welcome!

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m 'Add my feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

Please read our [Contributing Guidelines](CONTRIBUTING.md) and ensure all code follows our open-source licensing commitments.

---

## 📄 License

This project is licensed under the **GNU General Public License v2.0** — see the [LICENSE](LICENSE) file for details.

All code is open-source in alignment with UNICEF's Digital Public Goods principles and open-source licensing requirements.

---

## 🙏 Acknowledgements

- [UNICEF Office of Innovation](https://www.unicef.org/innovation/) — for championing blockchain solutions for children
- [OpenZeppelin](https://openzeppelin.com) — for battle-tested smart contract libraries
- [Polygon](https://polygon.technology) — for affordable, scalable Layer 2 infrastructure
- [Pinata](https://pinata.cloud) — for accessible IPFS storage
- The children, teachers, and communities of East Africa who inspired this platform

---

## 📬 Contact

**SomoHuru Team**  
📧 [hello@somohuru.com](mailto:hello@somohuru.com)  
🌍 Nairobi, Kenya  
🐦 [@SomoHuru](https://twitter.com/somohuru)  

---

<div align="center">
  <strong>🌍 Every child deserves a credential no one can take away.</strong>
</div>
