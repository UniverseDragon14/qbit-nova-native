# Pi 5 V3D GPU Routing Evidence — 2026-10-22

## Environment

- Platform: Raspberry Pi 5, Debian 13, aarch64
- Vulkan physical device: `V3D 7.1.7.0`
- Vendor ID: `0x14e4` (Broadcom)
- Build flags: `-std=c17 -O2 -Wall -Wextra -Wpedantic -Werror`

## Native source route

Command:

```bash
./build/qnova run examples/vector_add_u32.qn --backend auto
```

Result:

```text
qvm_requested_backend=auto
qvm_selected_backend=vulkan
qvm_selection_reason=verified-v3d-auto
qvm_operation=bounded-uint32-vector-add
qvm_gpu_eligible=true
qvm_gpu_execution_attempted=true
qvm_gpu_execution_completed=true
qvm_cpu_fallback=false
hardware_device=V3D 7.1.7.0
hardware_vendor_id=0x14e4
cpu_reference_validated=true
result_match=true
source_sha256=fd20313cbae8d5174d4655f396fe7090a0cdddaf11bd19939b47997de6da30df
qbc_sha256=9f9a624a88200847595c3a5499dc03fe4811bef1676e988fac4ce53a8448642f
shader_sha256=ae8c774d3923daf84f2ed729173226eab6923703997ccca5f718b1b3f47ae239
output_sha256=68a036d2c3ab163b9ff7ac887010b699fba71e94c89c29f4da908e08a9e5026b
```

## QBC execution route

The same source was compiled to a 104-byte QBC file, then executed with:

```bash
./build/qnova exec /tmp/qbit-nova-public-stage6-vector.qbc --backend auto
```

The QBC execution selected the same verified V3D backend, completed GPU execution, validated all 256 results against the CPU reference, and emitted the same output SHA-256. The compiled binary SHA-256 matched the independently built Stage 6 snapshot binary:

```text
binary_sha256=71d2fde0638080c0b00c5bda493a388e88ed3e5f5c538a7597d3ff656c1aa0e6
qbc_sha256=9f9a624a88200847595c3a5499dc03fe4811bef1676e988fac4ce53a8448642f
output_sha256=68a036d2c3ab163b9ff7ac887010b699fba71e94c89c29f4da908e08a9e5026b
```

## Verification boundary

The public-source integration's full `make test` suite passed. The bounded `uint32-vector-add` operation is routed through the physical V3D GPU on this Pi 5. This is not GPU acceleration of the quantum-state simulator, not a physical QPU, and not a quantum-speedup claim.
