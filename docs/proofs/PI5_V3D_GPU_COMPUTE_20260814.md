# Pi 5 V3D GPU Compute Evidence — 2026-08-14

## Result

A recorded Raspberry Pi 5 run used the Broadcom V3D integrated GPU through Vulkan for a bounded QBIT NOVA Native compute operation.

```text
QBIT_NOVA_NATIVE_V3D_COMPUTE=PASS
BOUNDARY=native_bounded_compute
PHYSICAL_QPU=false
BACKEND=vulkan
DEVICE=V3D 7.1.7.0
VENDOR=0x14e4
OPERATION=bounded-uint32-vector-add
ELEMENT_COUNT=256
GPU_EXECUTION_ATTEMPTED=true
GPU_EXECUTION_COMPLETED=true
CPU_FALLBACK=false
CPU_REFERENCE_VALIDATED=true
RESULT_MATCH=true
```

The recorded runtime receipt reported:

| Field | Value |
|---|---|
| QVM schema | `QBIT_NOVA_QVM_GPU_ROUTING_V06` |
| Requested backend | `vulkan` |
| Selected backend | `vulkan` |
| Selection reason | `explicit-verified-v3d` |
| Hardware device | `V3D 7.1.7.0` |
| Vendor ID | `0x14e4` |
| Source SHA-256 | `fd20313cbae8d5174d4655f396fe7090a0cdddaf11bd19939b47997de6da30df` |
| QBC SHA-256 | `9f9a624a88200847595c3a5499dc03fe4811bef1676e988fac4ce53a8448642f` |
| Shader SHA-256 | `ae8c774d3923daf84f2ed729173226eab6923703997ccca5f718b1b3f47ae239` |
| Output SHA-256 | `68a036d2c3ab163b9ff7ac887010b699fba71e94c89c29f4da908e08a9e5026b` |

The GPU result was checked against a CPU reference and matched.

## Separate V3D training validation

A second Raspberry Pi 5 hardware-validation run selected the same physical V3D GPU and executed one bounded linear-regression training step with a Vulkan compute shader.

```text
PHYSICAL_V3D_SELECTED=PASS
FORWARD_GPU=PASS
LOSS_GPU=PASS
BACKWARD_GPU=PASS
OPTIMIZER_GPU=PASS
WEIGHTS_CHANGED=PASS
LOSS_DECREASED=PASS
GPU_DISPATCH_COMPLETED=PASS
CPU_FALLBACK_USED=NO
LLVMPipe_USED=NO
QBIT_NOVA_REAL_GPU_TRAINING=PASS
```

The recorded loss changed from `36.000000000` to `15.681600571`, and the recorded weight file hash changed.

This training run is included as Raspberry Pi 5 V3D hardware evidence. It is not a claim that every QBIT NOVA Native program runs on the GPU.

## Relationship to the 200,000-shot GHZ proof

The separate [Pi 5 GHZ3 200,000-shot proof](./PI5_GHZ3_200K_20260825.md) used:

```text
BOUNDARY=software_virtual_qcpu
PHYSICAL_QPU=false
BACKEND=cpu
TOTAL=200000
000=100198
111=99802
INVALID_STATES=0
```

Therefore the two evidence records prove different things:

- the 200,000-shot GHZ record is a deterministic software quantum-simulation/QBC execution proof on the CPU;
- the V3D record is conventional bounded GPU compute/training evidence on the Raspberry Pi 5 GPU.

The GPU evidence must not be presented as physical quantum hardware, quantum acceleration of the 200,000-shot GHZ run, or quantum speedup.

## Public-source revalidation — 2026-10-22

The Stage 6 implementation is now synchronized into the public source tree. The integrated public-source snapshot was built with strict C17 flags and its complete `make test` suite passed on the Raspberry Pi 5. A fresh real-hardware run also completed:

```text
requested_backend=vulkan
selected_backend=vulkan
selection_reason=explicit-verified-v3d
hardware_device=V3D 7.1.7.0
hardware_vendor_id=0x14e4
element_count=256
gpu_execution_attempted=true
gpu_execution_completed=true
cpu_reference_validated=true
result_match=true
cpu_fallback=false
output_sha256=68a036d2c3ab163b9ff7ac887010b699fba71e94c89c29f4da908e08a9e5026b
```

The raw JSON receipt is [`PI5_V3D_GPU_COMPUTE_20261022.json`](./PI5_V3D_GPU_COMPUTE_20261022.json). It is evidence for this bounded vector-add operation only. Quantum-state simulation remains CPU-only in Stage 6; this is not a physical QPU or a quantum-speedup claim. Reproduction requires a compatible Raspberry Pi 5 V3D/Vulkan environment; CPU-only systems can run the CPU reference and regression tests but cannot reproduce physical GPU execution.
