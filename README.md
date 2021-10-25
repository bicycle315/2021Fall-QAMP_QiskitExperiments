# [QiskitExperiment](https://github.com/bicycle315/QiskitExperiment/blob/master/1.rst)

* [Running Composite Experiments on a Multi Qubit Backend](https://github.com/bicycle315/QiskitExperiment/tree/main/Composite%20Experiments)  
  1. Select the backend i want to characterize.  
  2. Get the information about the qubits in that backend.  
  3. Run Parallel Experimet to get the [Frequency](https://github.com/bicycle315/QiskitExperiment/blob/main/Composite%20Experiments/211015_Frequency%20on%20'Lima'%20Multi%20Qubit.ipynb), [T1](https://github.com/bicycle315/QiskitExperiment/blob/main/Composite%20Experiments/211005_T1%20on%20'Lima'%20Multi%20Qubit.ipynb), [T2](https://github.com/bicycle315/QiskitExperiment/blob/main/Composite%20Experiments/211006_T2%20on%20'Lima'%20Multi%20Qubits.ipynb) of all the qubits at once.  
  4. Get the results and images.   
 
-[This](https://github.com/qiskit-advocate/qamp-fall-21/files/7298443/Qiskit_Template_pdf.pdf) is a reference for my presentation on [qamp](https://github.com/qiskit-advocate/qamp-fall-21/issues/44).  
-Since I have an access to premium ibm backend, I also run these codes on [ibm_cairo](https://github.com/bicycle315/QiskitExperiment/blob/main/Composite%20Experiments/211015_CairoBackend.ipynb) backend to characterize multi qubits.    
-Follow this [**tutorial**](https://github.com/bicycle315/QiskitExperiment/blob/master/1.rst) (how to guide).
* [Qubit Frequency](https://github.com/bicycle315/QiskitExperiment/tree/main/Qubit%20Frequency) : can be found by two modules    
  * [Qiskit-Terra](https://github.com/bicycle315/QiskitExperiment/blob/main/Qubit%20Frequency/210928_FindingQbFreq%20.ipynb)  
    -> get the backend qubit's frequency  
    -> set the frequency sweep range  
    -> pulse.build as sweep_sched:  
      play Gaussian pulse to the qubit drive channel i want to know  
      measure the qubit  
    => Each schedule corresponds to a single frequncy point in the sweep range (schedules are produced as much as the number of frequency points)   
    => Each schedule is run as many times as i set "shot" and averaged.


  * [Qiskit Experiment](https://github.com/bicycle315/QiskitExperiment/blob/main/Qubit%20Frequency/210928_QubitSpectroscopy.ipynb)  
    -> get the backend qubit's frequency  
    -> set the frequency sweep range  
    -> run Spectroscopy!!  
    Spectroscopy gate is embedded with **qubit driving pulse + qubit measurement**  
    => **Spectroscopy is a circuit performing a spectroscopy gate with a different pulse schedule on a qubit**
    
* Following Tutorials
1. Following Single qubit gates on Armonk
2. Save Experiment data on the cloud
