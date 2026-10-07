# Foundry Upgrades

A Foundry-based implementation of the UUPS (Universal Upgradeable Proxy Standard) pattern. This project demonstrates how to deploy upgradeable smart contracts that preserve state across contract upgrades while maintaining security controls.

## What the product does
This project shows how to build a versioned contract system using the UUPS proxy pattern. It includes BoxV1 (a simple value-storage contract) and BoxV2 (an upgraded version with additional functionality), plus the proxy infrastructure needed to delegate calls while preserving contract state.

The flow is:
- deploy BoxV1 behind a proxy
- initialize storage
- upgrade to BoxV2 by authorizing a new implementation
- call new BoxV2 functions while keeping existing state

## The problem it solves
Once a smart contract is deployed, it cannot be changed. This creates a dilemma: bugs and missing features are permanent. An upgradeable proxy allows the implementation to be swapped out while preserving storage and user interaction patterns, enabling fixes and feature additions post-deployment.

## My specific contribution
I implemented the UUPS proxy pattern using OpenZeppelin's contracts, designed BoxV1 and BoxV2 contracts to demonstrate versioning, and created deployment and upgrade scripts. The focus is on showing how to safely upgrade while preserving state.

## Architecture
The repository includes:

- `src/BoxV1.sol` — initial implementation with a simple value storage
- `src/BoxV2.sol` — upgraded implementation with additional functionality
- `src/sublesson/` — supporting files demonstrating delegate call and proxy concepts
- `script/DeployBox.s.sol` — deployment script for the proxy and BoxV1
- `script/UpgradeBox.s.sol` — upgrade script to migrate from V1 to V2
- `test/DeployandUpgradeTest.t.sol` — tests for the upgrade flow
- `lib/` — OpenZeppelin upgradeable contracts and Foundry dependencies
- `foundry.toml` — Foundry configuration with remappings

## Technologies
- Solidity
- Foundry
- Forge testing
- OpenZeppelin upgradeable contracts
- ERC1967 proxy pattern
- UUPS upgrade mechanism

## Important technical decisions
- The UUPS pattern is used instead of transparent proxies to reduce gas costs and complexity.
- Both BoxV1 and BoxV2 use the Initializable pattern to prevent re-initialization after deployment.
- An `_authorizeUpgrade` function restricts upgrade calls to the contract owner.
- Identical storage layout is maintained between V1 and V2 to avoid state corruption.
- The proxy is deployed at a standard ERC1967 address pattern, enabling predictable interaction.

## Key features
- UUPS proxy infrastructure for safe upgrades
- Version-based contract migration with preserved state
- Owner-controlled upgrade authorization
- Initializer pattern for post-deployment setup
- Comprehensive upgrade tests
- Clear separation between proxy and implementation concerns

## Screenshots
No screenshots are included.

## Live demo
No live deployment is included in the repository.

## Challenges and solutions
The biggest challenge in upgradeable contracts is ensuring storage layout compatibility between versions. Changing the order or type of state variables causes data corruption during upgrade. This is solved by carefully designing BoxV2 to maintain the same storage layout as BoxV1.

Another challenge is preventing accidental re-initialization after deployment. The solution is using OpenZeppelin's Initializable pattern and the `initializer` modifier.

## Setup instructions
```bash
# Install Foundry
curl -L https://foundry.paradigm.xyz | bash
foundryup

# Clone
git clone https://github.com/Hayotunday/foundry-upgrades.git
cd foundry-upgrades

# Install dependencies
forge install

# Build
forge build

# Run tests
forge test

# Deploy BoxV1 and proxy
forge script script/DeployBox.s.sol --rpc-url <rpc-url> --private-key <private-key>

# Upgrade to BoxV2
forge script script/UpgradeBox.s.sol --rpc-url <rpc-url> --private-key <private-key>

# Optional
forge fmt
forge snapshot
```
