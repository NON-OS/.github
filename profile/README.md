<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.svg">
  <img alt="NØNOS. Zero-trust operating system. Built from bare metal." src="assets/banner-light.svg" width="100%">
</picture>

<br>

**NØNOS** is an operating system built from bare metal around one idea: a machine should trust as
little as possible and remember as little as it needs. The kernel starts from zero state, runs in
memory, and loads only software whose signature it can check. Every privilege is a capability
granted on purpose, not something a process inherits by accident.

The same idea runs through the rest of the stack. **The NØNOS STARK** is a transparent proof
system written from scratch: Goldilocks, FRI, Poseidon and Keccak, with no trusted setup and no
pairings. **NØNOS Shield** uses it to move value privately on Ethereum. A transfer is one
transaction, and inside it the chain verifies the whole proof and recomputes every constraint
itself.

<br>

## How it fits together

```mermaid
flowchart LR
  subgraph Host["Build host"]
    SRC["capsule source"] --> MK["nonos-mk<br/>package"]
    MK --> SIGN["nonos-sign<br/>Ed25519 + ML-DSA-65"]
  end
  subgraph Trust["Trust chain"]
    KS["nonos-trust-keystore<br/>anchor, policy, ledger"]
    ATT["stark-attest<br/>one 32-byte statement"]
  end
  subgraph Machine["The machine"]
    K["microkernel<br/>zero state, capabilities"]
    C1["capsule"]
    C2["capsule"]
  end
  SIGN --> KS
  SIGN --> ATT
  KS -->|verified at load| K
  K --> C1
  K --> C2
```

<br>

## Principles

**Zero state.** The system runs from memory and forgets on shutdown. Nothing persists unless it is
asked to.

**Capabilities, not ambient authority.** A process can do only what it has been handed an explicit
right to do.

**Everything signed.** Capsules carry a hybrid signature, classical and post-quantum. The kernel
checks it against a sealed trust anchor before a single instruction runs.

**Proofs over promises.** Where a claim can be checked by a machine, it is: by a signature, by a
transparent proof, or by a test that fails when the property breaks.

<br>

## NØNOS Shield

Private transfers on Ethereum L1, verified by a STARK in a single transaction.

<table>
  <tr>
    <td align="center" width="25%"><b>1 transaction</b><br><sub>per private transfer, with the whole proof verified on chain</sub></td>
    <td align="center" width="25%"><b>0 trusted setups</b><br><sub>no ceremony, no pairings, no off-chain verifier</sub></td>
    <td align="center" width="25%"><b>Every constraint</b><br><sub>recomputed by the chain, not supplied by the prover</sub></td>
    <td align="center" width="25%"><b>Sepolia</b><br><sub>first end-to-end private transfers settled, in ETH and NOX</sub></td>
  </tr>
</table>

```mermaid
flowchart LR
  D["deposit<br/>a note joins the pool"] --> P["wallet proves<br/>the transfer"]
  P --> S["one transaction<br/>STARK verified on chain"]
  S --> R["receiver<br/>finds and spends the note"]
  R --> W["withdrawal<br/>to any address"]
```

Blinded proofs, proving on your own device and an external audit come before mainnet.

<br>

## Repositories

<table>
  <tr>
    <th align="left" width="28%">Kernel</th>
    <td>
      <a href="https://github.com/NON-OS/microkernel"><b>microkernel</b></a> <sub>The NØNOS microkernel, in Rust</sub><br>
      <a href="https://github.com/NON-OS/nonos-docs"><b>nonos-docs</b></a> <sub>Kernel documentation</sub><br>
      <a href="https://github.com/NON-OS/micro-kernel-only-tests"><b>micro-kernel-only-tests</b></a> <sub>Boot smoke harnesses and recorded results</sub><br>
      <a href="https://github.com/NON-OS/nonos-ci"><b>nonos-ci</b></a> <sub>Static checks, baselines and trust-chain CI</sub><br>
      <a href="https://github.com/NON-OS/VBox"><b>VBox</b></a> <sub>Run NØNOS in VirtualBox</sub>
    </td>
  </tr>
  <tr>
    <th align="left">Trust chain</th>
    <td>
      <a href="https://github.com/NON-OS/nonos-trust-keystore"><b>nonos-trust-keystore</b></a> <sub>Trust anchor, publisher keys, sealed policy, signed manifests and ledger</sub><br>
      <a href="https://github.com/NON-OS/nonos-sign"><b>nonos-sign</b></a> <sub>Capsule signer and verifier, Ed25519 + ML-DSA-65</sub><br>
      <a href="https://github.com/NON-OS/nonos-mk"><b>nonos-mk</b></a> <sub>Capsule packager and marketplace index</sub><br>
      <a href="https://github.com/NON-OS/stark-attest"><b>stark-attest</b></a> <sub>Attest a set of artifacts with one 32-byte statement, transparently</sub>
    </td>
  </tr>
  <tr>
    <th align="left">Proofs</th>
    <td>
      <b>NØNOS STARK</b> <sub>The transparent prover and verifier: Goldilocks, FRI, DEEP, Poseidon. Opening at launch</sub><br>
      <b>NØNOS Shield</b> <sub>The shielded pool and its on-chain STARK verifier. Opening at launch</sub><br>
      <a href="https://github.com/NON-OS/zkolang"><b>zkolang</b></a> <sub>A verifiable-compute language, proven by the NØNOS transparent STARK</sub>
    </td>
  </tr>
  <tr>
    <th align="left">Apps and tools</th>
    <td>
      <a href="https://github.com/NON-OS/nonos.software"><b>nonos.software</b></a> <sub>Wiki, manual, ISO downloads and news</sub><br>
      <a href="https://github.com/NON-OS/android-app"><b>android-app</b></a> · <a href="https://github.com/NON-OS/ios-app"><b>ios-app</b></a> · <a href="https://github.com/NON-OS/NOXDashboard"><b>NOXDashboard</b></a> · <a href="https://github.com/NON-OS/NOXtools"><b>NOXtools</b></a>
    </td>
  </tr>
</table>

<br>

<p align="center">
  <a href="https://nonos.software"><b>nonos.software</b></a>
  &nbsp;·&nbsp;
  <a href="https://nonos.systems"><b>nonos.systems</b></a>
  &nbsp;·&nbsp;
  <a href="mailto:team@nonos.systems"><b>team@nonos.systems</b></a>
</p>

<p align="center"><sub><i>Trust nothing you cannot verify. Keep nothing you do not need.</i></sub></p>
