<div align="center">
  <img width="300" height="300" alt="logo" src="https://github.com/user-attachments/assets/8e12dfee-e289-48bb-a7ee-b2b1dea0198d" />
</div>

# ZKProofport

**Composable privacy infrastructure for credentials, applications, and AI agents.**

ZKProofport turns existing credentials into zero-knowledge proofs so applications can verify only the facts they need — without requiring users to reveal the underlying wallet, email, country, identity data, or other private inputs.

We build the stack openly, from proof specifications and Noir circuits to EVM verifiers, mobile proving, SDKs, and agent-native interfaces.

```text
Credential / Attestation
        ↓
CIP Specification
        ↓
Reference Noir Circuit
        ↓
ZK Proof
        ↓
EVM Verifier
        ↓
App / Protocol / Agent
```

## Open Credential Proof Stack

ZKProofport is organized into several layers with different responsibilities.

| Layer                     | Purpose                                                                                                 | Repository                                                              |
| ------------------------- | ------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **CIPs**                  | Open specifications describing what a proof means, what remains private, and what must be trusted       | [`CIPs`](https://github.com/zkproofport/CIPs)                           |
| **Circuits**              | Noir reference implementations, tests, Solidity verifiers, and deployment records                       | [`circuits`](https://github.com/zkproofport/circuits)                   |
| **Mobile Proving**        | On-device proof generation using React Native and mopro                                                 | [`proofport-app`](https://github.com/zkproofport/proofport-app)         |
| **App SDK**               | Request proofs from the mobile app and verify results on-chain                                          | [`proofport-app-sdk`](https://github.com/zkproofport/proofport-app-sdk) |
| **Agent Infrastructure**  | Agent-native proof generation through MCP/A2A, ERC-8004 identity, x402 payments, and TEE infrastructure | [`proofport-ai`](https://github.com/zkproofport/proofport-ai)           |
| **Reference Application** | ZK-gated community for humans and AI agents                                                             | [`openstoa`](https://github.com/zkproofport/openstoa)                   |

## Circuit Improvement Proposals

**Circuit Improvement Proposals (CIPs)** are project-level open RFCs for privacy-preserving credential proofs.

A CIP describes:

* what statement is proven;
* what data remains private;
* the credential source and trust anchor;
* public and private input semantics;
* nullifier and scope behavior;
* verifier requirements; and
* known security and trust assumptions.

CIPs are inspired by the EIP process, but they are specifications for this project rather than Ethereum standards.

| CIP                                                                    | Proof Profile                         | Status | Implementation |
| ---------------------------------------------------------------------- | ------------------------------------- | ------ | -------------- |
| [`CIP-1`](https://github.com/zkproofport/CIPs/blob/main/CIPS/cip-1.md) | Coinbase KYC Attestation              | Review | Reference      |
| [`CIP-2`](https://github.com/zkproofport/CIPs/blob/main/CIPS/cip-2.md) | Coinbase Country Predicate            | Review | Reference      |
| [`CIP-3`](https://github.com/zkproofport/CIPs/blob/main/CIPS/cip-3.md) | OIDC Domain Attestation               | Review | Reference      |
| [`CIP-4`](https://github.com/zkproofport/CIPs/blob/main/CIPS/cip-4.md) | GIWA Dojang Verified Address          | Draft  | PoC            |
| [`CIP-5`](https://github.com/zkproofport/CIPs/blob/main/CIPS/cip-5.md) | Korean Mobile ID Selective Disclosure | Draft  | Experimental   |

### What can currently be proven?

**Coinbase KYC**

Prove control of an address referenced by a Coinbase attestation while keeping the source address, transaction, and signatures private.

**Coinbase Country**

Prove that a private country value satisfies a public inclusion or exclusion policy without revealing the actual country.

**OIDC Domain**

Prove Google Workspace or Microsoft 365 organization-domain membership without revealing the user's email or JWT.

**GIWA Dojang**

A GIWA Sepolia PoC for converting a Verified Address-style attestation into a privacy-preserving proof. The production profile will evolve with mainnet issuer, schema, and EAS parameters.

**Korean Mobile ID**

Experimental circuits for selected-field commitment, age threshold, and region predicates. The next step is connecting these predicates to a canonical Mobile ID issuer and source-authentication flow.

## Ethereum-Native Verification

Reference UltraHonk verifier contracts are deployed across EVM networks.

Current reference deployments include:

* **Ethereum Sepolia** — Coinbase KYC, Country, OIDC
* **Base Mainnet** — Coinbase KYC, Country, OIDC
* **Base Sepolia** — Coinbase KYC, Country, OIDC, Korean Mobile ID predicates
* **GIWA Sepolia** — GIWA attestation PoC

Deployment records and exact contract addresses are maintained in the [`circuits`](https://github.com/zkproofport/circuits) repository.

The generated Solidity verifiers expose a common interface:

```solidity
function verify(
    bytes calldata proof,
    bytes32[] calldata publicInputs
) external view returns (bool);
```

This makes credential proofs composable with Ethereum applications, access control, governance, payments, and financial logic.

## Private Proofs on Mobile

[`proofport-app`](https://github.com/zkproofport/proofport-app) is our open-source React Native prover.

It integrates **mopro**, developed by Ethereum's Privacy & Scaling Explorations ecosystem, with project-specific adaptations for generating Noir/UltraHonk proofs on mobile devices.

The app currently supports proof flows for Coinbase KYC, Country, OIDC, and experimental Korean Mobile ID predicates.

For client-side flows, private witness data can remain on the user's device while applications receive only the proof and its defined public inputs.

## SDK for Applications

[`@zkproofport-app/sdk`](https://github.com/zkproofport/proofport-app-sdk) provides a simple way for applications to request proofs from the mobile prover.

```text
Application
    ↓ proof request
SDK / Relay
    ↓ deep link or QR
ZKProofport App
    ↓ on-device proving
Proof + Public Inputs
    ↓
EVM Verification
```

The SDK is one way to consume CIP-compatible proofs; the underlying specifications and verifier contracts remain independently usable.

## Privacy for AI Agents

[`proofport-ai`](https://github.com/zkproofport/proofport-ai) explores the same privacy primitives for autonomous agents.

The agent stack includes:

* MCP and A2A interfaces for proof requests;
* ERC-8004-based agent identity;
* x402-based payment flows;
* AWS Nitro Enclave-based trusted execution for agent-side proving workflows; and
* EVM-verifiable proof results.

The goal is to let an agent answer narrow questions such as:

> Has this user completed KYC?
> Is the user from an allowed country?
> Does the user belong to this organization?

without requiring the agent to receive the user's full credential.

## OpenStoa

[`OpenStoa`](https://github.com/zkproofport/openstoa) is a reference application built on the ZKProofport stack.

It is a ZK-gated community where humans and AI agents can participate using privacy-preserving proofs for access and topic eligibility.

OpenStoa supports Coinbase KYC, Country, Google Workspace, and Microsoft 365 proof-gated topics, with selected content recorded on Base and MLS-based end-to-end encrypted chat.

🏅 **1st Place — The Synthesis Hackathon, “Agents That Keep Secrets”**

## Why Open Specifications?

Credential circuits are security-sensitive.

A proof is only useful if developers can understand:

* exactly what was proven;
* what information was hidden;
* which issuer, key, contract, or root was trusted; and
* what the circuit does **not** prove.

That is why we publish both the specifications and their reference implementations.

Our goal is to make these circuits easy to inspect, reproduce, challenge, and improve — and to increase independent review as the project and contributor community grow.

The current reference circuits have not completed an independent external security audit. Security assumptions and known limitations are documented in each CIP.

## Where We Are Going

We started with identity and credential proofs, but the same model can extend to other private predicates.

Examples include:

* proving an asset or balance threshold without revealing the exact holdings or source wallet;
* proving age, region, residency, or organization membership;
* using private eligibility proofs as conditions for RWA participation;
* gating stablecoin transfers or other on-chain actions on verified private predicates; and
* enabling agents to purchase and compose proofs without accessing raw user credentials.

The long-term goal is simple:

**Make trustworthy off-chain facts privately composable with Ethereum.**

## Recognition

* **Base Batches 002** — Top 50
* **Aztec / Noir** — Coinbase KYC Noir PoC implementation support
* **The Synthesis Hackathon** — OpenStoa, 1st Place in *Agents That Keep Secrets*

## Links

|                 |                                                                                              |
| --------------- | -------------------------------------------------------------------------------------------- |
| 🌐 Website      | [zkproofport.com](https://zkproofport.com)                                                   |
| 📋 CIPs         | [github.com/zkproofport/CIPs](https://github.com/zkproofport/CIPs)                           |
| ⚙️ Circuits     | [github.com/zkproofport/circuits](https://github.com/zkproofport/circuits)                   |
| 📱 Mobile App   | [github.com/zkproofport/proofport-app](https://github.com/zkproofport/proofport-app)         |
| 📦 SDK          | [github.com/zkproofport/proofport-app-sdk](https://github.com/zkproofport/proofport-app-sdk) |
| 🤖 Prover Agent | [github.com/zkproofport/proofport-ai](https://github.com/zkproofport/proofport-ai)           |
| 🏛️ OpenStoa    | [github.com/zkproofport/openstoa](https://github.com/zkproofport/openstoa)                   |

## Contributing

We welcome review, implementation feedback, new credential profiles, test vectors, and security analysis.

For new circuit proposals, start with:

* [`CIP-0`](https://github.com/zkproofport/CIPs/blob/main/CIPS/cip-0.md)
* [`CONTRIBUTING.md`](https://github.com/zkproofport/CIPs/blob/main/CONTRIBUTING.md)

For implementation issues and verifier work, use the corresponding repository.

## License

CIP specifications are released under **CC0**.

Reference implementations and application repositories use the licenses declared in their respective repositories.
