# QAOA for MaxCut

> **Category**: optimization &nbsp;|&nbsp; **Difficulty**: advanced &nbsp;|&nbsp; **Qubits**: 4 &nbsp;|&nbsp; **Gates**: 20 &nbsp;|&nbsp; **Depth**: 10

QAOA is a variational hybrid quantum-classical algorithm for combinatorial optimization. It alternates between a problem Hamiltonian (phase separator) and a mixing Hamiltonian. This circuit implements p=1 QAOA for the MaxCut problem on a 4-node cycle graph (edges: 0-1, 1-2, 2-3, 3-0) with γ=0.5 (phase) and β=0.3 (mixing). The parameters should be classically optimized to maximize the expected cut value.

## Expected Output

Probability distribution peaked on good MaxCut partitions (depends on γ, β)

## Circuit

The OpenQASM 2.0 circuit is in [`circuit.qasm`](./circuit.qasm).

```
OPENQASM 2.0;
include "qelib1.inc";
// QAOA p=1 for MaxCut on 4-cycle: nodes {0,1,2,3}, edges {01,12,23,30}
// gamma=0.5, beta=0.3 (example parameters; classical optimiser should tune these)
qreg q[4];
creg c[4];
// Initial state: uniform superposition
h q[0]; h q[1]; h q[2]; h q[3];
// Phase separator: exp(-i*gamma/2 * ZZ) on each edge
// Edge (0,1)
cx q[0],q[1]; rz(0.5) q[1]; cx q[0],q[1];
// Edge (1,2)
cx q[1],q[2]; rz(0.5) q[2]; cx q[1],q[2];
// Edge (2,3)
cx q[2],q[3]; rz(0.5) q[3]; cx q[2],q[3];
// Edge (3,0)
cx q[3],q[0]; rz(0.5) q[0]; cx q[3],q[0];
// Mixing operator: exp(-i*beta * X) = Rx(2*beta) on each qubit
rx(0.6) q[0]; rx(0.6) q[1]; rx(0.6) q[2]; rx(0.6) q[3];
measure q[0] -> c[0];
measure q[1] -> c[1];
measure q[2] -> c[2];
measure q[3] -> c[3];
```

## Tags

`qaoa` `optimization` `maxcut` `variational` `combinatorial`

## References

- [Farhi, Goldstone, Gutmann (2014). A Quantum Approximate Optimization Algorithm. arXiv:1411.4028](https://arxiv.org/abs/1411.4028)

## License

MIT — part of the [OpenQC Algorithm Catalog](https://github.com/openqc-algorithms).
