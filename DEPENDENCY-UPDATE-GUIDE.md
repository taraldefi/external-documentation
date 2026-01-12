# Taral Ecosystem - Dependency Update Guide

## Overview

This guide provides step-by-step instructions for updating the Taral ecosystem dependencies from their current versions to the latest stable releases. The repositories affected are:

- **taraldefi/taral** - Main monorepo
- **taraldefi/taral-bank** - Smart contracts
- **taraldefi/nft-marketplace** - NFT marketplace

> **Note:** These repositories are currently archived. This guide should be used when forking or unarchiving the projects.

---

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Node.js Update](#2-nodejs-update)
3. [Clarinet SDK Migration (v1 → v3)](#3-clarinet-sdk-migration-v1--v3)
4. [Stacks.js Migration (v6 → v7)](#4-stacksjs-migration-v6--v7)
5. [Vitest Migration (v1 → v4)](#5-vitest-migration-v1--v4)
6. [Vite Migration (v5 → v6)](#6-vite-migration-v5--v6)
7. [TypeScript Update](#7-typescript-update)
8. [Post-Update Verification](#8-post-update-verification)

---

## 1. Prerequisites

### System Requirements

```bash
# Check current Node.js version
node --version

# Required: Node.js 18+ (20 LTS recommended)
# Install via nvm:
nvm install 20
nvm use 20
```

### Backup

```bash
# Create a backup branch before starting
git checkout -b backup/pre-dependency-update
git push origin backup/pre-dependency-update
git checkout -b feature/dependency-updates
```

---

## 2. Node.js Update

### Current State
- Required: Node.js 14+
- **Target: Node.js 18+ (20 LTS recommended)**

### Update package.json

```json
{
  "engines": {
    "node": ">=18.0.0"
  }
}
```

### Update CI/CD Workflows

In `.github/workflows/*.yml`:

```yaml
# Before
- uses: actions/setup-node@v3
  with:
    node-version: '14'

# After
- uses: actions/setup-node@v4
  with:
    node-version: '20'
```

### Docker Updates

Update Dockerfiles:

```dockerfile
# Before
FROM node:14-alpine

# After
FROM node:20-alpine
```

---

## 3. Clarinet SDK Migration (v1 → v3)

### References
- [Official Migration Guide](https://docs.hiro.so/stacks/clarinet-js-sdk/guides/migrate-to-the-clarinet-sdk)
- [NPM Package](https://www.npmjs.com/package/@hirosystems/clarinet-sdk)
- [GitHub Releases](https://github.com/hirosystems/clarinet/releases)

### Installation

```bash
# Update Clarinet SDK
npm install @hirosystems/clarinet-sdk@latest

# Current latest: 3.5.0 (as of August 2025)
```

### Breaking Changes

#### 1. Test Command Changed

```bash
# Before (deprecated)
clarinet test

# After
npm test
```

#### 2. Simnet Initialization

```typescript
// Before (v1)
import { Clarinet, Tx, Chain, Account, types } from '@hirosystems/clarinet-sdk';

Clarinet.test({
  name: "test name",
  async fn(chain: Chain, accounts: Map<string, Account>) {
    // test code
  }
});

// After (v3)
import { initSimnet } from '@hirosystems/clarinet-sdk';
import { Cl } from '@stacks/transactions';
import { describe, it, expect, beforeEach } from 'vitest';

describe('Contract Tests', () => {
  let simnet: Awaited<ReturnType<typeof initSimnet>>;

  beforeEach(async () => {
    simnet = await initSimnet();
  });

  it('should do something', () => {
    const result = simnet.callPublicFn(
      'contract-name',
      'function-name',
      [Cl.uint(100)],
      simnet.deployer
    );
    expect(result.result).toBeOk(Cl.bool(true));
  });
});
```

#### 3. Global Simnet Object

The `simnet` object is now available globally in tests and is automatically initialized before each test.

```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    environment: 'clarinet',
    singleThread: true,
    setupFiles: ['./tests/setup.ts'],
  },
});
```

#### 4. Clarity Value Helpers

```typescript
// Before
import { types } from '@hirosystems/clarinet-sdk';
types.uint(100);
types.principal('ST...');

// After
import { Cl } from '@stacks/transactions';
Cl.uint(100);
Cl.principal('ST...');
Cl.standardPrincipal('ST...');
Cl.contractPrincipal('ST...', 'contract-name');
Cl.tuple({ key: Cl.uint(1) });
Cl.list([Cl.uint(1), Cl.uint(2)]);
```

### Migration Script

Create `scripts/migrate-clarinet-tests.sh`:

```bash
#!/bin/bash

# Find all test files
find tests -name "*.ts" -type f | while read file; do
  echo "Processing: $file"

  # Replace imports
  sed -i 's/from "@hirosystems\/clarinet-sdk"/from "@hirosystems\/clarinet-sdk"\nimport { Cl } from "@stacks\/transactions"/g' "$file"

  # Replace types.uint -> Cl.uint
  sed -i 's/types\.uint/Cl.uint/g' "$file"
  sed -i 's/types\.int/Cl.int/g' "$file"
  sed -i 's/types\.bool/Cl.bool/g' "$file"
  sed -i 's/types\.principal/Cl.principal/g' "$file"
  sed -i 's/types\.ascii/Cl.stringAscii/g' "$file"
  sed -i 's/types\.utf8/Cl.stringUtf8/g' "$file"
  sed -i 's/types\.buff/Cl.buffer/g' "$file"
  sed -i 's/types\.list/Cl.list/g' "$file"
  sed -i 's/types\.tuple/Cl.tuple/g' "$file"
  sed -i 's/types\.some/Cl.some/g' "$file"
  sed -i 's/types\.none/Cl.none/g' "$file"
  sed -i 's/types\.ok/Cl.ok/g' "$file"
  sed -i 's/types\.err/Cl.err/g' "$file"
done

echo "Migration complete. Please review changes manually."
```

---

## 4. Stacks.js Migration (v6 → v7)

### References
- [GitHub Migration Guide](https://github.com/hirosystems/stacks.js/blob/main/.github/MIGRATION.md)
- [Hiro Blog: Announcing Stacks.js v7](https://www.hiro.so/blog/announcing-stacks-js-v7)
- [NPM Package](https://www.npmjs.com/package/@stacks/transactions)

### Installation

```bash
# Update all Stacks.js packages together
npm install @stacks/transactions@latest @stacks/network@latest @stacks/common@latest
```

### Breaking Changes

#### 1. Network Objects (CRITICAL)

```typescript
// Before (v6)
import { StacksMainnet, StacksTestnet, StacksDevnet } from '@stacks/network';

const network = new StacksMainnet();
const testnet = new StacksTestnet();
const devnet = new StacksDevnet();

// After (v7)
import { STACKS_MAINNET, STACKS_TESTNET, STACKS_DEVNET } from '@stacks/network';

// These are static objects, not classes
const network = STACKS_MAINNET;
const testnet = STACKS_TESTNET;
const devnet = STACKS_DEVNET;
```

#### 2. Serialization Returns String (CRITICAL)

```typescript
// Before (v6)
const serialized: Uint8Array = transaction.serialize();

// After (v7)
const serializedHex: string = transaction.serialize();
// If you need bytes:
const serializedBytes: Uint8Array = transaction.serializeBytes();
```

#### 3. ClarityType Enum

```typescript
// Before (v6)
import { ClarityType } from '@stacks/transactions';
if (value.type === ClarityType.UInt) { }

// After (v7)
import { ClarityType, ClarityWireType } from '@stacks/transactions';
// Human-readable version
if (value.type === ClarityType.UInt) { }
// Wire format (for low-level operations)
if (value.type === ClarityWireType.UInt) { }
```

#### 4. Post Conditions

```typescript
// Before (v6)
import {
  makeStandardSTXPostCondition,
  FungibleConditionCode
} from '@stacks/transactions';

const postCondition = makeStandardSTXPostCondition(
  address,
  FungibleConditionCode.Equal,
  1000000n
);

// After (v7)
import { Pc } from '@stacks/transactions';

const postCondition = Pc.principal(address).willSendEq(1000000n).ustx();
// Or for tokens:
const tokenPostCondition = Pc.principal(address)
  .willSendLte(100n)
  .ft('contract.token-name', 'token-id');
```

#### 5. StacksTransaction Class

```typescript
// Before (v6)
import { StacksTransaction } from '@stacks/transactions';

// After (v7)
import { StacksTransactionWire } from '@stacks/transactions';
// Constructor now takes options object
```

#### 6. Fetch Methods Renamed

```typescript
// Before (v6)
import {
  getAccountBalance,
  getContractInfo
} from '@stacks/transactions';

// After (v7)
import {
  fetchAccountBalance,
  fetchContractInfo
} from '@stacks/transactions';
```

#### 7. Asset Info Type Renamed

```typescript
// Before (v6)
import { AssetInfo, createAssetInfo } from '@stacks/transactions';

// After (v7)
import { Asset, createAsset } from '@stacks/transactions';
```

#### 8. Structured Data Encoding

```typescript
// Before (v6)
const encoded: Uint8Array = encodeStructuredData(data);

// After (v7)
const encodedHex: string = encodeStructuredData(data);
// If you need bytes:
const encodedBytes: Uint8Array = encodeStructuredDataBytes(data);
```

#### 9. Contract Deploys Default to Clarity 3

```typescript
// Before (v6) - defaulted to Clarity 2
makeContractDeploy({ ... });

// After (v7) - defaults to Clarity 3
// To use Clarity 2 explicitly:
makeContractDeploy({
  clarityVersion: 2,
  ...
});
```

### Migration Script

Create `scripts/migrate-stacks-v7.sh`:

```bash
#!/bin/bash

find . -name "*.ts" -o -name "*.tsx" | while read file; do
  echo "Processing: $file"

  # Network changes
  sed -i 's/new StacksMainnet()/STACKS_MAINNET/g' "$file"
  sed -i 's/new StacksTestnet()/STACKS_TESTNET/g' "$file"
  sed -i 's/new StacksDevnet()/STACKS_DEVNET/g' "$file"

  # Import changes
  sed -i 's/StacksMainnet, StacksTestnet/STACKS_MAINNET, STACKS_TESTNET/g' "$file"

  # AssetInfo -> Asset
  sed -i 's/AssetInfo/Asset/g' "$file"
  sed -i 's/createAssetInfo/createAsset/g' "$file"

  # Fetch method renames
  sed -i 's/getAccountBalance/fetchAccountBalance/g' "$file"
  sed -i 's/getContractInfo/fetchContractInfo/g' "$file"
  sed -i 's/getNonce/fetchNonce/g' "$file"
  sed -i 's/getAbi/fetchAbi/g' "$file"
done

echo "Migration complete. Review serialization changes manually!"
```

---

## 5. Vitest Migration (v1 → v4)

### References
- [Official Migration Guide](https://vitest.dev/guide/migration.html)
- [Vitest 4.0 Announcement](https://vitest.dev/blog/vitest-4)
- [GitHub Release Notes](https://github.com/vitest-dev/vitest/releases/tag/v4.0.0)

### Installation

```bash
npm install vitest@latest @vitest/coverage-v8@latest
```

### Breaking Changes

#### 1. Pool Options Restructured (CRITICAL)

```typescript
// Before (v1)
// vitest.config.ts
export default defineConfig({
  test: {
    poolOptions: {
      threads: {
        singleThread: true,
        useAtomics: true,
      },
      vmThreads: {
        memoryLimit: '512MB',
      },
    },
  },
});

// After (v4)
export default defineConfig({
  test: {
    singleThread: true,  // Moved to top level
    // useAtomics removed entirely
    vmMemoryLimit: '512MB',  // Renamed
  },
});
```

#### 2. Coverage Configuration (CRITICAL)

```typescript
// Before (v1)
export default defineConfig({
  test: {
    coverage: {
      all: true,
      ignoreEmptyLines: true,
    },
  },
});

// After (v4)
export default defineConfig({
  test: {
    coverage: {
      // 'all' removed - define include/exclude instead
      include: ['src/**/*.ts'],
      exclude: ['**/*.test.ts', '**/*.d.ts'],
    },
  },
});
```

#### 3. Mock Behavior Changes

```typescript
// Before (v1)
vi.restoreAllMocks(); // Restored all mocks including vi.fn()

// After (v4)
vi.restoreAllMocks(); // Only restores vi.spyOn() mocks
// For vi.fn() mocks, use:
vi.resetAllMocks();
```

#### 4. Invocation Order

```typescript
// Before (v1)
const mock = vi.fn();
mock();
expect(mock.mock.invocationCallOrder[0]).toBe(0);

// After (v4)
const mock = vi.fn();
mock();
expect(mock.mock.invocationCallOrder[0]).toBe(1); // Starts at 1
```

#### 5. Workspace → Projects

```typescript
// Before (v1-v3)
// vitest.workspace.ts

// After (v4)
// Use 'projects' in vitest.config.ts
export default defineConfig({
  test: {
    projects: ['packages/*'],
  },
});
```

#### 6. Reporter API Changes

```typescript
// Before
{
  reporters: ['basic'],
  onCollected: () => {},
}

// After
{
  reporters: ['default'],
  summary: false,  // For basic-like output
  // onCollected removed - use alternative hooks
}
```

#### 7. CLI Flag Changes

```bash
# Before
vitest --minWorkers 2

# After (removed)
# Configure in vitest.config.ts instead
```

### Updated Configuration

```typescript
// vitest.config.ts (v4 compatible)
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    globals: true,
    environment: 'node',
    include: ['tests/**/*.test.ts'],
    exclude: ['node_modules', 'dist'],
    coverage: {
      provider: 'v8',
      include: ['src/**/*.ts'],
      exclude: ['**/*.test.ts', '**/*.d.ts', '**/types/**'],
      reporter: ['text', 'json', 'html'],
    },
    singleThread: true,  // For Clarinet tests
    testTimeout: 30000,
    hookTimeout: 30000,
  },
});
```

---

## 6. Vite Migration (v5 → v6)

### References
- [Vite Migration Guide](https://vite.dev/guide/migration)

### Installation

```bash
npm install vite@latest
```

### Breaking Changes

#### 1. Node.js 18+ Required

Vite 6 requires Node.js 18+. Ensure your environment meets this requirement.

#### 2. Environment API Changes

```typescript
// Before (v5)
import { createServer } from 'vite';

// After (v6) - mostly compatible, check plugins
```

#### 3. CSS Processing

```typescript
// vite.config.ts
export default defineConfig({
  css: {
    // Review CSS processing options
    preprocessorOptions: {
      scss: {
        // Updated API
      },
    },
  },
});
```

---

## 7. TypeScript Update

### Installation

```bash
npm install typescript@latest
```

### Configuration Updates

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

## 8. Post-Update Verification

### Step 1: Clean Install

```bash
# Remove old dependencies
rm -rf node_modules
rm package-lock.json  # or yarn.lock

# Fresh install
npm install
```

### Step 2: Type Check

```bash
npm run typecheck
# or
npx tsc --noEmit
```

### Step 3: Run Tests

```bash
npm test
```

### Step 4: Build

```bash
npm run build
```

### Step 5: Lint

```bash
npm run lint
```

### Verification Checklist

- [ ] All packages installed without conflicts
- [ ] TypeScript compilation succeeds
- [ ] All unit tests pass
- [ ] All integration tests pass
- [ ] Build completes successfully
- [ ] No console warnings about deprecated APIs
- [ ] Docker images build successfully
- [ ] CI/CD pipeline passes

---

## Common Issues & Solutions

### Issue 1: Type Errors After Network Update

```typescript
// Error: Type 'StacksMainnet' is not assignable...

// Solution: Use the static objects
import { STACKS_MAINNET } from '@stacks/network';
```

### Issue 2: Serialize Returns String

```typescript
// Error: Type 'string' is not assignable to type 'Uint8Array'

// Solution: Use serializeBytes() if you need bytes
const bytes = transaction.serializeBytes();
```

### Issue 3: Pool Options Not Recognized

```typescript
// Error: Unknown option 'poolOptions'

// Solution: Move options to top level in vitest.config.ts
{
  test: {
    singleThread: true,  // Not poolOptions.threads.singleThread
  }
}
```

### Issue 4: Coverage.all Not Working

```typescript
// Warning: 'all' option is deprecated

// Solution: Use include/exclude patterns
{
  coverage: {
    include: ['src/**/*.ts'],
  }
}
```

### Issue 5: Clarinet Test Command Not Found

```bash
# Error: clarinet test is deprecated

# Solution: Use npm test with vitest
npm test
```

---

## Quick Reference: Package Versions

| Package | From | To | Breaking? |
|---------|------|----|-----------|
| @hirosystems/clarinet-sdk | ^1.0.4 | ^3.5.0 | Yes |
| @stacks/transactions | ^6.8.1 | ^7.3.0 | Yes |
| @stacks/network | ^6.x | ^7.x | Yes |
| vitest | ^1.0.4 | ^4.0.x | Yes |
| vitest-environment-clarinet | ^1.0.2 | ^latest | Yes |
| vite | ^5.0.2 | ^6.x | Minor |
| typescript | 5.2.2 | 5.7.x | No |
| Node.js | 14+ | 18+ | Yes |

---

## Additional Resources

- [Hiro Documentation](https://docs.hiro.so/)
- [Stacks.js Documentation](https://stacks.js.org/)
- [Vitest Documentation](https://vitest.dev/)
- [Vite Documentation](https://vite.dev/)
- [Clarinet GitHub](https://github.com/hirosystems/clarinet)
