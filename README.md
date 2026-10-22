<p align="center">
  <img
    src="assets/qbit-nova-logo.jpg"
    alt="QBIT NOVA"
    width="720"
  />
</p>

# QBIT NOVA Native

**Native C17 programming language, deterministic QBC/QVM runtime, software virtual QCPU, and approval-first execution security.**

> Research Preview
> Created by Universal Dragon Aslam

## Truth boundary

```text
physical_qpu=false
runtime=software_virtual_qcpu
python_dependency=false
release_class=research_preview
```

QBIT NOVA does not claim that Raspberry Pi 5 or ordinary software becomes a physical quantum computer.

It currently implements quantum-state simulation and a secure native execution architecture.

## Pi5 evidence and branch boundary

The [Pi5 GHZ3 200,000-shot record](docs/proofs/PI5_GHZ3_200K_20260825.md) reports exact source/QBC histogram agreement on the V10 Guardian development branch: 100,198 × `000`, 99,802 × `111`, and zero invalid states.

The public `examples/ghz3.qn` SHA-256 exactly matches the source used by that run. The Stage 6 bounded V3D compute proof and QVM GPU-routing implementation are now included in this public source tree. Stage 7 typed-runtime and V10 data/Guardian implementation files remain development-branch work. The evidence document separates source identity from compiler-branch identity.

## Native pipeline

```text
QBIT NOVA source
→ deterministic lexer
→ parser
→ typed AST
→ typed QIR
→ QBC bytecode
→ QVM
→ capability guard
→ authenticated approval
→ trusted issuer
→ replay protection
→ bounded execution
→ evidence receipt
```

## Current features

- Native C17 implementation
- Deterministic lexer, parser, QIR, QBC, and QVM
- Software state-vector quantum simulation
- Deny-by-default capability guard
- OpenSSL Ed25519 signed approvals
- Strict trusted-issuer store
- Atomic persistent replay ledger
- Replay protection for `run` and `exec`
- Strict token and issuer revocation-store foundation
- Deterministic execution results and receipts
- `shell.exec` blocked by default

## Stage 6 GPU routing and V3D compute

The native runtime includes a deny-by-default Vulkan adapter, a bounded `uint32-vector-add` V3D compute proof, and truthful QVM backend routing. Build and run the full suite with `make test`.

- `qnova gpu compute-proof --backend vulkan --receipt proof.json` requires verified Broadcom V3D hardware and fails closed if the required device/driver contract is not met.
- `qnova run` / `qnova exec` accept `--backend cpu|auto|vulkan`. `auto` routes the bounded vector-add operation to verified V3D when available. Quantum-state simulation remains CPU-only in Stage 6; explicit Vulkan for that ineligible operation fails closed rather than silently claiming GPU execution.
- The bounded V3D kernel is a GPU compute proof, not a quantum-state GPU simulator or physical QPU.
- `docs/GPU_ADAPTER_CONTRACT_V06.md`, `docs/GPU_COMPUTE_PROOF_V06.md`, `docs/GPU_QVM_ROUTING_V06.md`, and `docs/BOUNDED_GPU_OPERATION_V06.md` document the scope and constraints.
- Fresh Pi 5 hardware receipts: [`V3D compute`](docs/proofs/PI5_V3D_GPU_COMPUTE_20261022.json) and [`source/QBC auto-routing`](docs/proofs/PI5_V3D_GPU_ROUTING_20261022.md).

## Security status

| Layer | Status |
|---|---|
| Ed25519 approvals | Implemented |
| Trusted issuer store | Implemented |
| Replay ledger | Implemented |
| Run replay protection | Implemented |
| Exec replay protection | Implemented |
| Token revocation lookup | Implemented |
| Issuer revocation lookup | Implemented |
| Revocation execution wiring | Implemented; regression-tested |

## Build and test

Requirements:

- C17 compiler
- GNU Make
- OpenSSL development package
- `pkg-config`

```bash
make
make test
```

Strict build flags:

```text
-std=c17
-O2
-Wall
-Wextra
-Wpedantic
-Werror
```

## Project status

This repository is an active research preview.

It is not yet a production security product, physical quantum computer, or final QBIT NOVA release.

## License

No open-source license has been assigned yet.

Public visibility does not automatically grant redistribution, modification, or commercial-use rights.
