# Taral Platform - Product Requirements Document (PRD)

## Document Information
- **Version:** 1.0
- **Date:** 2026-01-12
- **Status:** Draft
- **Author:** Generated from grant analysis and repository review

---

## Executive Summary

Taral is a DeFi platform built on the Stacks blockchain designed to democratize international trade finance by replacing traditional bank-intermediated Letter of Credit processes with smart contracts. This PRD documents the current state of the project, identifies outdated dependencies, and outlines unfinished features requiring implementation.

---

## 1. Outdated Dependencies Analysis

### 1.1 Critical Updates Required

| Package | Current Version | Latest Version | Severity | Notes |
|---------|-----------------|----------------|----------|-------|
| @hirosystems/clarinet-sdk | ^1.0.4 | 3.5.0 | **Critical** | Major version jump, breaking changes expected |
| @stacks/transactions | ^6.8.1 | 7.3.0 | **Critical** | Major version, recommended by Hiro docs |
| vitest | ^1.0.4 | 4.0.16 | **High** | Major version, significant test framework changes |
| vite | ^5.0.2 | 6.x | **High** | Major version update available |
| typescript | 5.2.2 | 5.7.x | **Medium** | Minor version updates with new features |
| ts-node | ^10.9.2 | 10.9.x | **Low** | Minor updates only |
| Node.js requirement | 14+ | 18+ | **Critical** | Clarinet SDK 3.x requires Node.js 18+ |

### 1.2 Ecosystem Updates

| Component | Status | Notes |
|-----------|--------|-------|
| Clarity version | 2.0 | Current, but check for 3.0 updates |
| Clarinet CLI | Unknown | Should update to latest (2.x+) |
| Stacks.js ecosystem | 7.x | Full ecosystem update recommended |

### 1.3 Dependency Update Commands

```bash
# Update Clarinet SDK and Stacks transactions
npm install @hirosystems/clarinet-sdk@latest @stacks/transactions@latest

# Update testing framework
npm install vitest@latest vite@latest vitest-environment-clarinet@latest

# Update TypeScript
npm install typescript@latest
```

---

## 2. Unfinished Features from Grant Milestones

### 2.1 Grant Milestone 1 ($13,350) - Core Transaction Features

| ID | User Story | Estimation (hrs) | Status | Priority |
|----|-----------|------------------|--------|----------|
| 1 | Submit purchase order details | 140 | **Unverified** | P0 |
| 2 | View and sign/disapprove purchase contract | 19 | **Unverified** | P0 |
| 7 | Input and approve purchase terms with importer | 29 | **Unverified** | P0 |
| 19 | Web boilerplate foundation | 34 | **Partial** | P1 |
| 14 | Review underlying assets for liquidity provision | 45 | **Unverified** | P1 |

**Total Milestone 1:** 267 hours

### 2.2 Grant Milestone 2 ($6,250) - Payment & Delivery Features

| ID | User Story | Estimation (hrs) | Status | Priority |
|----|-----------|------------------|--------|----------|
| 3 | Select payment terms and repayment duration | 13 | **Not Started** | P0 |
| 5 | Confirm receipt of shipment and release funds | 53 | **Not Started** | P0 |
| 9 | Upload proof of delivery/shipment | 59 | **Not Started** | P1 |

**Total Milestone 2:** 125 hours

---

## 3. Platform Features - Detailed Requirements

### 3.1 Purchase Order Management System

#### 3.1.1 Purchase Order Submission (User Story #1)

**Description:** Enable buyers to submit detailed purchase orders on-chain.

**Functional Requirements:**
- FR-1.1: Form to capture purchase order details (buyer info, seller info, goods description, quantity, unit price, total value, currency)
- FR-1.2: Validate all required fields before submission
- FR-1.3: Generate unique purchase order ID
- FR-1.4: Store purchase order data on-chain via Clarity smart contract
- FR-1.5: Emit events for purchase order creation
- FR-1.6: Display confirmation with transaction hash

**Technical Requirements:**
- Smart contract: `purchase-order.clar`
- Data map for purchase orders with proper indexing
- Integration with Stacks wallet (Hiro Wallet, Leather)
- Frontend form validation with TypeScript types

**Acceptance Criteria:**
- [ ] User can fill and submit purchase order form
- [ ] Purchase order is stored on Stacks blockchain
- [ ] Transaction confirmation is displayed
- [ ] Purchase order is retrievable by ID

---

#### 3.1.2 Purchase Contract Viewing & Signing (User Story #2)

**Description:** Enable parties to view, review, and digitally sign purchase contracts.

**Functional Requirements:**
- FR-2.1: Display purchase contract details in readable format
- FR-2.2: Show all parties involved and their roles
- FR-2.3: Enable digital signature via wallet transaction
- FR-2.4: Support disapproval with reason field
- FR-2.5: Track signature status for all parties
- FR-2.6: Notify parties of signature/approval status changes

**Technical Requirements:**
- Multi-signature smart contract logic
- Signature verification on-chain
- Status tracking (pending, signed, rejected)
- Event emission for status changes

**Acceptance Criteria:**
- [ ] Both buyer and seller can view contract
- [ ] Signing creates verifiable on-chain record
- [ ] Disapproval captures reason and notifies parties
- [ ] Contract status accurately reflects all signatures

---

#### 3.1.3 Purchase Terms Input & Approval (User Story #7)

**Description:** Allow parties to input, negotiate, and approve purchase terms.

**Functional Requirements:**
- FR-7.1: Form to input purchase terms (payment terms, delivery terms, incoterms, insurance requirements)
- FR-7.2: Version history for term modifications
- FR-7.3: Approval workflow for both parties
- FR-7.4: Lock terms once both parties approve
- FR-7.5: Display terms summary before final approval

**Technical Requirements:**
- Clarity map for terms with versioning
- State machine for approval workflow
- Read-only functions for term retrieval

---

### 3.2 Payment & Delivery System

#### 3.2.1 Payment Terms Selection (User Story #3)

**Description:** Enable buyers to select when payment should be released and repayment duration.

**Functional Requirements:**
- FR-3.1: Select payment release trigger (on shipment, on delivery, custom milestone)
- FR-3.2: Choose repayment duration (30/60/90 days)
- FR-3.3: Calculate and display interest rates based on duration
- FR-3.4: Show total repayment amount
- FR-3.5: Lock payment terms in smart contract

**Technical Requirements:**
- Interest calculation logic (Clarity math)
- Integration with lending pool for rate determination
- Collateral ratio calculation based on terms

---

#### 3.2.2 Shipment Confirmation & Fund Release (User Story #5)

**Description:** Enable buyers to confirm receipt of goods, triggering automatic fund release to sellers.

**Functional Requirements:**
- FR-5.1: Display shipment status and tracking (if available)
- FR-5.2: Confirmation button for goods receipt
- FR-5.3: Automatic fund transfer to seller upon confirmation
- FR-5.4: Dispute mechanism before confirmation
- FR-5.5: Timeout mechanism for auto-release (configurable)
- FR-5.6: Partial confirmation for split shipments

**Technical Requirements:**
- Escrow smart contract functionality
- STX/stablecoin transfer logic
- Time-based release mechanism using block height
- Event emission for fund transfers

**Acceptance Criteria:**
- [ ] Buyer can confirm goods receipt
- [ ] Funds automatically transfer to seller
- [ ] Transaction is recorded on-chain
- [ ] Dispute can be raised before confirmation

---

#### 3.2.3 Proof of Delivery Upload (User Story #9)

**Description:** Enable sellers to upload proof of delivery/shipment documentation.

**Functional Requirements:**
- FR-9.1: File upload interface (PDF, images)
- FR-9.2: Document type selection (bill of lading, delivery receipt, customs docs)
- FR-9.3: Store document hash on-chain
- FR-9.4: Off-chain storage for actual documents (IPFS/Arweave)
- FR-9.5: Link documents to specific purchase order
- FR-9.6: Document verification status

**Technical Requirements:**
- IPFS/Arweave integration for document storage
- SHA-256 hash generation and on-chain storage
- Document metadata in Clarity contract

---

### 3.3 Liquidity & Investment Features

#### 3.3.1 Asset Review for Liquidity Provision (User Story #14)

**Description:** Enable investors to review underlying assets before providing liquidity.

**Functional Requirements:**
- FR-14.1: List all available trade finance opportunities
- FR-14.2: Display risk metrics per transaction
- FR-14.3: Show collateral details (NFT-based)
- FR-14.4: Expected yield calculation
- FR-14.5: Historical performance data
- FR-14.6: Filter and sort by risk/return profile

**Technical Requirements:**
- Read-only contract functions for asset data
- Aggregation of transaction history
- Risk scoring algorithm

---

### 3.4 Advanced Platform Features (Post-Grant)

These features were outlined in the project vision but not included in grant milestones:

#### 3.4.1 Insurance Pool

**Priority:** P2

**Requirements:**
- Pool for protecting against defaults
- Premium calculation based on transaction risk
- Claim processing mechanism
- Investor returns from insurance premiums

#### 3.4.2 Lending Pool

**Priority:** P2

**Requirements:**
- Working capital loans (30/60/90 day durations)
- Variable yield based on collateral and risk
- Loan origination and repayment flows
- Interest accrual mechanism

#### 3.4.3 NFT Collateral System

**Priority:** P1

**Requirements:**
- Mint NFT representing transaction data
- Use NFT as collateral for loans
- Transfer/burn on transaction completion
- Integration with NFT marketplace (existing)

#### 3.4.4 Oracle Integration

**Priority:** P2

**Requirements:**
- Price feeds for on-chain assets
- Custom oracle implementation (initial)
- Chainlink integration (when available on Stacks)
- Stablecoin price verification

#### 3.4.5 DAO Governance

**Priority:** P3

**Requirements:**
- TAL token for governance
- Proposal submission and voting
- Treasury management
- Protocol parameter updates

---

## 4. Technical Architecture

### 4.1 Smart Contract Structure

```
contracts/
├── core/
│   ├── purchase-order.clar        # Purchase order management
│   ├── purchase-contract.clar     # Contract signing logic
│   ├── escrow.clar                # Fund escrow and release
│   └── payment-terms.clar         # Payment term configurations
├── pools/
│   ├── insurance-pool.clar        # Insurance pool logic
│   └── lending-pool.clar          # Lending pool logic
├── tokens/
│   ├── tal-token.clar             # TAL governance token
│   └── collateral-nft.clar        # NFT collateral
├── governance/
│   └── dao.clar                   # DAO governance
└── utils/
    ├── oracle.clar                # Price oracle
    └── storage.clar               # Shared storage patterns
```

### 4.2 Frontend Architecture

```
apps/portal/
├── pages/
│   ├── orders/                    # Purchase order management
│   ├── contracts/                 # Contract viewing/signing
│   ├── payments/                  # Payment management
│   ├── investor/                  # Investor dashboard
│   └── governance/                # DAO interface
├── components/
│   ├── forms/                     # Form components
│   ├── wallet/                    # Wallet integration
│   └── shared/                    # Shared UI components
└── services/
    ├── stacks/                    # Stacks blockchain integration
    └── storage/                   # IPFS/document storage
```

---

## 5. Implementation Phases

### Phase 1: Foundation (Weeks 1-4)
- Dependency updates
- Smart contract scaffolding
- Web boilerplate completion
- Wallet integration

### Phase 2: Core Transactions (Weeks 5-10)
- Purchase order submission
- Contract viewing and signing
- Purchase terms workflow

### Phase 3: Payment & Delivery (Weeks 11-16)
- Payment terms selection
- Escrow implementation
- Fund release mechanism
- Proof of delivery system

### Phase 4: Investment Features (Weeks 17-22)
- Asset review interface
- NFT collateral system
- Basic pool architecture

### Phase 5: Advanced Features (Weeks 23+)
- Insurance pool
- Lending pool
- Oracle integration
- DAO governance

---

## 6. Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Purchase orders created | 100+ | On-chain count |
| Successful fund releases | 95%+ | Completion rate |
| Average transaction time | <24 hrs | Block confirmation to release |
| User wallet connections | 500+ | Unique addresses |
| Total value locked | $100k+ | Pool balances |

---

## 7. Risk Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Smart contract vulnerabilities | Medium | Critical | Unit tests, audits, Clarity safety features |
| Oracle manipulation | Medium | High | Multi-source oracles, circuit breakers |
| Low liquidity adoption | High | Medium | Incentive programs, partnerships |
| Regulatory uncertainty | Medium | High | Legal consultation, geo-fencing |
| Stacks ecosystem changes | Low | Medium | Monitor updates, abstract dependencies |

---

## 8. References

- [Taral Main Repository](https://github.com/taraldefi/taral)
- [Taral Bank Smart Contracts](https://github.com/taraldefi/taral-bank)
- [Taral NFT Marketplace](https://github.com/taraldefi/nft-marketplace)
- [@hirosystems/clarinet-sdk NPM](https://www.npmjs.com/package/@hirosystems/clarinet-sdk)
- [@stacks/transactions NPM](https://www.npmjs.com/package/@stacks/transactions)
- [Vitest NPM](https://www.npmjs.com/package/vitest)
- [Hiro Clarinet Documentation](https://docs.hiro.so/stacks/clarinet-js-sdk/installation)
