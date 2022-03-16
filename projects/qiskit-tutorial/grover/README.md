# Grover's Algorithm / Amplitude Amplification

## About

This is one of the most famous quantum algorithms.
Was introduced in 1996 by Lov Grover.
It was proposed for unstructured search problems, like finding a marked ellement in an unstructured database.
However, now Grover's algorithm is a subroutine to several other algorithms, such as Adaptive Search

In a nutshell, Grover’s algorithm applies different powers of Q and after each execution checks whether a good solution has been found.

## Qiskit

The implementation is in the class `Grover`, that includes:
- The generalized version
- Amplitude Amplification
- Set of individual iterations
- Other meta-settings to Grover's algorithm

The oracle for Grover must be a *phase-flip oracle*. That is, it multiplies the amplitudes of the of “good states” by a factor of `−1`

```py
from qiskit import QuantumCircuit
from qiskit.algorithms import AmplificationProblem

# the state we desire to find is '11'
good_state = ['11']

# specify the oracle that marks the state '11' as a good solution
oracle = QuantumCircuit(2)
oracle.cz(0, 1)

# define Grover's algorithm
problem = AmplificationProblem(oracle, is_good_state=good_state)

# now we can have a look at the Grover operator that is used in running the algorithm
# (Algorithm circuits are wrapped in a gate to be appear in composition as a block
# so we have to decompose() the op to see it expanded into its component gates.)
problem.grover_operator.decompose().draw(output='mpl')
```

If the search was successful, the `oracle_evaluation` attribute of the result will be `True`. In this case, the most sampled measurement, `top_measurement`, is one of the "good states". Otherwise, `oracle_evaluation` will be `False`.

```py
from qiskit import Aer
from qiskit.utils import QuantumInstance
from qiskit.algorithms import Grover

aer_simulator = Aer.get_backend('aer_simulator')
grover = Grover(quantum_instance=aer_simulator)
result = grover.amplify(problem)
print('Result type:', type(result))
print()
print('Success!' if result.oracle_evaluation else 'Failure!')
print('Top measurement:', result.top_measurement)
```

In the example, the result of `top_measurement` is 11 which is one of "good state". Thus, we succeeded to find the answer by using `Grover`.

### Different Types of Classes as the Oracle

...
