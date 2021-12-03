================================================
Running a Composite Experiment on Multi Qubits
================================================
There are information about qubit frequencies,T1, T2 and other properties corresponding to
all qubits for each backends
at `IBM Quantum Service. <https://quantum-computing.ibm.com/services?services=systems.>`_ 
We are going to choose the backend for experiment , characterize frequency, T1, T2 and 
compare whether the results are similar to the reported ones.


1. T1 Characterization
=======================
Choose the backend you want to characterize.
For people who do not have access to the premium ibm backend services, they can try 
ibmq_santigo, bogota, belem, manila, quito and lima backend which have more than one qubits. 
In this guide, we will try ibmq_lima. 
Then, find out how many qubits are in our backend 
and all T1 values corresponding to our qubits afterwards. 

.. code-block:: python

    IBMQ.load_account()
    provider = IBMQ.get_provider(hub='ibm-q', group='open', project='main')
    backend = provider.get_backend('ibmq_lima')


.. code-block:: python

    from qiskit_experiments.framework import ParallelExperiment
    from qiskit_experiments.library import T1

Get the basic features including qubit number of the backend.

.. code-block:: python

    config = backend.configuration()

    print("This backend is called {0}, and is on version {1}. It has {2} qubit{3}. It "
         "{4} OpenPulse programs. The basis gates supported on this device are {5}."
        "".format(config.backend_name, 
                config.backend_version, 
                config.n_qubits, 
                '' if config.n_qubits == 1 else 's',
                'supports' if config.open_pulse else 'does not support',
                config.basis_gates))

Import required packages for Composite Experiments.

.. code-block:: python

    from qiskit_experiments.framework import ParallelExperiment
    from qiskit_experiments.library import T1

    backend = backend
    delays = list(range(1, 150, 5))

    exps=[]
    for i in range(config.n_qubits):
        exp = T1(qubit=i,
                delays=delays,
                unit="us")
    exps.append(exp)  
 print(exps)

 parallel_exp = ParallelExperiment(exps)
 parallel_data = parallel_exp.run(backend, shots=8192).block_for_results()

Now we will view the result data.

.. code-block:: python

    for result in parallel_data.analysis_results():
        print(result)
        print("\nextra:")
        print(result.extra)

Finally, let's get every sub-experiment data and figures.

.. code-block:: python

    for i in range(parallel_exp.num_experiments):
        print(f"Component experiment {i}")
        sub_data = parallel_data.component_experiment_data(i)
        display(sub_data.figure(0))
        for result in sub_data.analysis_results():
            print(result)

2.T2* and Ramsey Characterization
===================================
We will continue to use the lima backend for our T2 characterization.
In this Experiment, we will get T2* and Ramsey frequency as a result data.
Start by defining sub experiments.

.. code-block:: python

    T2_exps=[]
    delays = list(range(1, 150, 5))

    for i in range(config.n_qubits):
        exp = T2Ramsey(qubit=i,
                delays=delays,
                unit="us",
                  osc_freq=1e4)
    exp.set_analysis_options(plot=True)
    T2_exps.append(exp)
   
    print(T2_exps)

    # print corresponding circuits to experiments to see how it consists of.
    print(exp.circuits()[3])

.. code-block:: python

    # default shots is set at 1024 and the maximum shots we can try is 8192.
    # choose the shots number according to your required accuracy.
    parallel_exp = ParallelExperiment(T2_exps)
    parallel_data = parallel_exp.run(backend, shots=8192).block_for_results()

Now let's see the result data and each of sub-experiment data

.. code-block:: python

    for result in parallel_data.analysis_results():
        print(result)
        print("\nextra:")
        print(result.extra)

    # print sub-experiment data
    for i in range(parallel_exp.num_experiments):
        print(f"Component experiment {i}")
    sub_data = parallel_data.component_experiment_data(i)
    display(sub_data.figure(0))
    for result in sub_data.analysis_results():
        print(result)

3. Finding qubits with Qubit Spectroscopy
=========================================
We will sweep the frequency around the known qubit frequency to see the resonance 
at the qubit frequency reported by the backend. 

.. code-block:: python

    backend = backend

    exps=[]
    for i in range(config.n_qubits):
    
        freq_estimate = backend.defaults().qubit_freq_est[i]
        frequencies = np.linspace(freq_estimate -15e6, freq_estimate + 15e6, 51)
        exp = QubitSpectroscopy(i, frequencies)
            
        exps.append(exp)

    print(exps)

Check how the spectroscopy experiment is constructed by drawing circuits.

.. code-block:: python

    circuit_Q0 = exp.circuits(backend)[0]
    circuit_Q0.draw(output="mpl")

Now, lets construct a parallel experiment to get the frequencies of multiple qubits.

.. code-block:: python

    parallel_exp = ParallelExperiment(exps)
    parallel_data = parallel_exp.run(backend, shots=8192).block_for_results()


.. code-block:: python

    # View result data
    for result in parallel_data.analysis_results():
        print(result)
        print("\nextra:")
        print(result.extra)

    # Print sub-experiment data
    for i in range(parallel_exp.num_experiments):
        print(f"Component experiment {i}")
        sub_data = parallel_data.component_experiment_data(i)
        display(sub_data.figure(0))
    for result in sub_data.analysis_results():
        print(result)
