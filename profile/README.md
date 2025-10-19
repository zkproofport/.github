# zkProofport — General-Purpose ZK Proof Portal

zkProofport generates zero-knowledge proofs in the browser and delivers them to your dApp via postMessage. The portal requests only the inputs required by each circuit spec and does not force unnecessary account connections.

## Key Features

* Circuit-agnostic: given Noir metadata (JSON) and bytecode, the portal can execute the circuit client-side
* Simple integration: `openPortal({ circuit })` to launch, receive `{ proof, publicInputs, meta }`
* Dual verification paths: off-chain (UltraHonk) or on-chain (Verifier contract)
* Minimal exposure: proof generation happens in browser memory; only the result is posted back once

## First circuit in preparation

* Coinbase KYC (in development)

  * Circuit ID: `coinbase_kyc`
  * Inputs: based on on-chain transaction/signature context
  * Verification: off-chain (UltraHonk) and on-chain (Base Verifier)

## Upcoming circuit examples

* Email/domain affiliation proof: verify organizational membership from an email or issued token without revealing the address
* Social follower threshold proof: verify “follower count ≥ N” via hashed OAuth snapshot and ZK proof, without exposing raw data

These circuits request only the minimum inputs required by their specs. Wallet connection is currently relevant only to the Coinbase KYC circuit.

## dApp integration example

```ts
import { openPortal, verifyOffchain, verifyOnchain } from '@zkproofport/sdk';
import { JsonRpcProvider } from 'ethers';

const payload = await openPortal({ circuit: 'coinbase_kyc' });

const okOff = await verifyOffchain({
  proofHex: payload.proof,
  publicInputs: payload.publicInputs,
  circuitUrl: 'https://raw.githubusercontent.com/hsy822/zk-coinbase-attestor/develop/packages/circuit/target/zk_coinbase_attestor.json',
  threads: 1,
  keccak: true,
});

const okOn = await verifyOnchain({
  proof: payload.proof,
  publicInputs: payload.publicInputs,
  verifierAddress: '0xB3705B6d33Fe7b22e86130Fa12592B308a191483',
  provider: new JsonRpcProvider(process.env.NEXT_PUBLIC_BASE_RPC_URL),
});
```

## Security principles

* Minimal input collection: only what the circuit requires
* Session metadata: includes origin, nonce, timestamp to prevent replay and validate source
* Cross-validation: compare off-chain and on-chain results for consistency where applicable

## Links

* Portal: [https://zkproofport.com/portal](https://zkproofport.com/portal)
* Demo: [https://proofport-demo.netlify.app/](https://proofport-demo.netlify.app/)
* NPM: [@zkproofport/sdk](https://www.npmjs.com/package/@zkproofport/sdk)
* GitHub: [https://github.com/zkproofport](https://github.com/zkproofport)
* Coinbase-KYC Verifier (Base): [0xB3705B6d33Fe7b22e86130Fa12592B308a191483](https://basescan.org/address/0xB3705B6d33Fe7b22e86130Fa12592B308a191483#code)
