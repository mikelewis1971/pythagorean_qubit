# pythagorean_qubit

Appendix: Complete Python & Qiskit Code
This appendix contains all the essential programs used in the course, categorized by topic.

1. Installing and Setting Up Qiskit
Before running any of the quantum circuits, ensure you have Qiskit installed. Run the following command in your terminal:

pip install qiskit
To set up a virtual environment (recommended):

python -m venv quantum_env
source quantum_env/bin/activate  # On Mac/Linux
quantum_env\Scripts\activate     # On Windows
pip install qiskit
Now you can proceed with writing and running quantum circuits.

2. Creating a Basic Quantum Circuit
This example initializes a 5-qubit system and applies a Hadamard gate to each qubit, creating an equal superposition.

from qiskit import QuantumCircuit, Aer, execute

# Create a quantum circuit with 5 qubits
qc = QuantumCircuit(5, 5)

# Apply Hadamard gates to all qubits to create superposition
for qubit in range(5):
    qc.h(qubit)

# Measure all qubits
qc.measure(range(5), range(5))

# Visualize the circuit
print(qc.draw())

# Simulate the circuit
simulator = Aer.get_backend('qasm_simulator')
result = execute(qc, simulator, shots=1024).result()
counts = result.get_counts()

# Print results
print("\nMeasurement results:", counts)
3. Implementing the Curvature-Corrected Formula
This function computes the modified Pythagorean theorem for both the + and - cases.

import math

def classical_distance(a, b, R, plus=True):
    """Compute the curvature-corrected hypotenuse."""
    correction = (a**2 * b**2) / (R**2)
    if plus:
        c_squared = a**2 + b**2 + correction
    else:
        c_squared = a**2 + b**2 - correction
    return math.sqrt(c_squared)

# Example usage
a, b, R = 3, 4, 10
print("Hyperbolic case (+):", classical_distance(a, b, R, plus=True))
print("Spherical case (-):", classical_distance(a, b, R, plus=False))
4. Encoding the 32-State System in a Quantum Circuit
This circuit represents the 5-qubit system where each qubit corresponds to a binary choice in our modified equation.

from qiskit import QuantumRegister, ClassicalRegister, QuantumCircuit

qreg = QuantumRegister(5, 'q')
creg = ClassicalRegister(5, 'c')
qc = QuantumCircuit(qreg, creg)

# Apply Hadamard gates to initialize superposition of all 32 states
qc.h(qreg)

# Measure all qubits
qc.measure(qreg, creg)

# Visualize the circuit
print(qc.draw())
5. Phase Shifts and Interference
Here, we apply phase shifts to control how different qubit states interfere.

from qiskit.circuit.library import U1Gate

qc = QuantumCircuit(5, 5)

# Apply Hadamard to all qubits
qc.h(range(5))

# Apply a phase shift to the last qubit (simulating a curvature effect)
phase_angle = 0.75  # Example value
qc.append(U1Gate(phase_angle), [4])

# Measure all qubits
qc.measure(range(5), range(5))

# Draw the circuit
print(qc.draw())
6. Custom Chiral Shift Operator
This example constructs a custom unitary gate to model a cyclic energy shift.

import numpy as np
from qiskit.extensions import UnitaryGate

theta = np.pi / 4
custom_matrix = np.array([[np.cos(theta), -np.sin(theta)],
                          [np.sin(theta),  np.cos(theta)]])
custom_gate = UnitaryGate(custom_matrix, label="ChiralShift")

qc = QuantumCircuit(5, 5)
qc.append(custom_gate, [4])  # Apply to the last qubit

qc.measure(range(5), range(5))
print(qc.draw())
7. Running the Circuit and Visualizing Results
To execute and visualize the quantum simulation, use:

from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

# Run the simulation
simulator = Aer.get_backend('qasm_simulator')
result = execute(qc, simulator, shots=1024).result()
counts = result.get_counts()

# Plot the results
plot_histogram(counts)
plt.show()
8. Implementing a Time Evolution Model
This script evolves the system over multiple iterations.

qc = QuantumCircuit(5, 5)

# Apply Hadamard gates
qc.h(range(5))

# Simulate energy evolution over 5 cycles
for _ in range(5):
    qc.rx(np.pi/4, range(5))  # Apply rotations as time steps

qc.measure(range(5), range(5))

# Run and plot
result = execute(qc, simulator, shots=1024).result()
counts = result.get_counts()
plot_histogram(counts)
plt.show()
9. Noise Simulation and Decoherence
To simulate the effect of noise, use Qiskit's built-in noise models.

from qiskit.providers.aer.noise import NoiseModel

noise_model = NoiseModel.from_backend(Aer.get_backend('qasm_simulator'))

# Run the simulation with noise
result = execute(qc, Aer.get_backend('qasm_simulator'), noise_model=noise_model, shots=1024).result()
counts = result.get_counts()

# Plot noisy results
plot_histogram(counts)
plt.show()
10. Classical-Quantum Hybrid Simulation
This merges classical geometry computations with quantum processing.

def quantum_curvature_simulation(a, b, R):
    """Map classical results onto a quantum system."""
    plus_c = classical_distance(a, b, R, plus=True)
    minus_c = classical_distance(a, b, R, plus=False)

    qc = QuantumCircuit(5, 5)
    qc.h(range(5))

    if plus_c > minus_c:
        qc.x(4)  # Encode classical preference in quantum state

    qc.measure(range(5), range(5))
    return qc

qc = quantum_curvature_simulation(3, 4, 10)
print(qc.draw())
Summary of the Code Appendix
Topics Covered
✔ Basic quantum circuits
✔ Superposition and phase shifts
✔ Simulating interference effects
✔ Custom unitary transformations
✔ Time evolution and cyclic energy shifts
✔ Hybrid classical-quantum modeling
✔ Noise models and real-world simulations

This appendix provides ready-to-use code for all major exercises and projects from the course. The Python and Qiskit scripts allow you to explore how quantum circuits simulate complex energy dynamics, interference, and emergent behavior.
