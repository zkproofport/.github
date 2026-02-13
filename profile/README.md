<div align="center">
  <img width="300" height="300" alt="logo" src="https://github.com/user-attachments/assets/8e12dfee-e289-48bb-a7ee-b2b1dea0198d" />
</div>

# ZKProofport — Privacy-First ZK Proof Infrastructure

ZKProofport is an **infrastructure and process** for zero-knowledge proofs. Through a circuit registry, proof portals, and SDKs, any type of ZK proof can be composed, generated, and verified — without exposing personal data.

## Architecture

```
Circuit Registry → Proof Portal (Web / Mobile / Agent) → Relay → On-Chain Verification → SDK
```

| Component | Description | Status |
|-----------|-------------|--------|
| **Web Portal** | Browser-based ZK proof generation via iframe SDK | Live |
| **Mobile Portal** | Client-side proving on device via mopro (Rust + Barretenberg) | Beta |
| **Prover Agent** | AI agent proving via ERC-8004 identity + x402 payments + TEE | In Development |
| **Relay Server** | Real-time proof request routing (Socket.IO + Redis) | Live |
| **On-Chain Verifiers** | UltraHonk verification contracts on EVM chains | Deployed (Base Sepolia) |
| **Nullifier Registry** | Sybil-resistant nullifier tracking on-chain | Deployed |

## Circuits (CIPs)

Circuits follow the [CIP (Circuit Improvement Proposal)](https://github.com/zkproofport/CIPs) standard — an open governance for ZK circuit standards, inspired by [EIPs](https://github.com/ethereum/EIPs).

| CIP | Circuit | Category | Status |
|-----|---------|----------|--------|
| [CIP-1](https://github.com/zkproofport/CIPs/blob/main/CIPS/cip-1.md) | Coinbase KYC Attestation | Identity | Review |
| [CIP-2](https://github.com/zkproofport/CIPs/blob/main/CIPS/cip-2.md) | Coinbase Country Attestation | Identity | Review |

**Circuit language**: Noir (Aztec)  
**Proof system**: Barretenberg UltraHonk  
**Mobile proving**: mopro (Rust FFI)

## Key Design Decisions

- **Client-side proving**: Proofs are generated on user's device (browser or mobile), never on a server
- **Circuit-agnostic infrastructure**: The portal, relay, and SDK work with any Noir circuit
- **Nullifier-based sybil resistance**: Each proof generates a scoped nullifier registered on-chain
- **Dual verification**: Off-chain (`@aztec/bb.js`) and on-chain (Verifier contracts)

## SDK Integration

```ts
import { ProofportSDK } from '@zkproofport-app/sdk';

const sdk = new ProofportSDK({
  defaultCallbackUrl: 'https://myapp.com/verify'
});

// Request a proof via mobile deep link or QR code
const request = sdk.createProofRequest({
  circuit: 'coinbase_attestation',
  scope: 'myapp.xyz'
});
const { deepLink, qrDataUrl } = await sdk.requestProof(request);
```

## On-Chain Contracts (Base Sepolia)

| Contract | Address |
|----------|---------|
| CoinbaseAttestation Verifier | [`0xEb9eb5452790Cfe549fF83CEB3Dbe1C432231492`](https://sepolia.basescan.org/address/0xEb9eb5452790Cfe549fF83CEB3Dbe1C432231492) |
| CoinbaseCountryAttestation Verifier | [`0xD0F3eE648386B59B484157332E736388Fcc41F47`](https://sepolia.basescan.org/address/0xD0F3eE648386B59B484157332E736388Fcc41F47) |
| Nullifier Registry | [`0xC6a8dC34B1872a883aFCc808C90c31c038764d9a`](https://sepolia.basescan.org/address/0xC6a8dC34B1872a883aFCc808C90c31c038764d9a) |

## Grants & Recognition

- **Base Batches 002** — Top 50 out of 900+ teams (Builder Track), currently incubating
- **Aztec Noir Grant** — Circuit development funding

## Links

| | |
|---|---|
| 🌐 Portal | [zkproofport.com](https://zkproofport.com) |
| 🤖 Prover Agent | [proveragent.eth.limo](https://proveragent.eth.limo) |
| 📋 CIPs | [github.com/zkproofport/CIPs](https://github.com/zkproofport/CIPs) |
| 📦 SDK (App) | [@zkproofport-app/sdk](https://www.npmjs.com/package/@zkproofport-app/sdk) |
| 📦 SDK (Web) | [@zkproofport/sdk](https://www.npmjs.com/package/@zkproofport/sdk) |

## License

All CIPs are released under [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
