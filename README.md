# Quantum Advantage in Machine Learning: Investigating Entanglement and Quantum Feature Maps in Neural Networks

[![License](https://img.shields.io/github/license/Qiskit/qiskit-machine-learning.svg?style=popout-square)](https://opensource.org/licenses/Apache-2.0)

Repository containing the code and final report for my Semester Project titled *"Quantum Advantage in Machine Learning: Investigating Entanglement and Quantum Feature Maps in Neural Networks"* carried out by [Umberto Borso](https://www.linkedin.com/in/umberto-borso-1990a11a1/) at ETH Scalable Parallel Computing Lab (SPCL) completed in Spring 2023 under the supervision of [Dr. Maciej Besta](https://people.inf.ethz.ch/bestam/) (ETH SPCL) and [Prof. Renato Renner](www.itp.phys.ethz.ch/people/renner/) (ETH Quantum Infromation Theory Group).

The full thesis report can be found [here](Umberto_Borso_Project_Report.pdf).

### Table of contents


- [Abstract](#abstract)
- [Code](#code)
  - [Installation](#installation)
  - [Heavy `.npy` files](#heavy-npy-files)
  - [Notebooks](#notebooks)
  - [License](#license)
- [References](#references)

## Abstract

This work explores different design choices of quantum neural network (QNN) models, focusing on the effect of quantum feature maps and variational circuits on model performance. Their impact on QNN models in terms of capacity and trainability is systematically evaluated using the Fisher information spectrum and the effective dimension, as proposed by Abbas et al. [[1]](#1).

<div align="center">
    <img height="300" src="Figures/QNNstructure.png">
</div>


Furthermore, the Meyer-Wallach measure, as suggested by Sim et al. [[2]](#2), is employed to quantify the entangling capability of the analysed quantum feature maps and variational circuits. Considering that entanglement is thought to play a pivotal role in achieving quantum advantage [[3]](#3), it is of the uttermost interest to systematically explore this aspect. This investigation aims at understanding the interplay between entanglement descriptors and QNN performance metrics, thereby laying the groundwork for a theoretical framework essential for characterising these key components in quantum machine learning.

<div align="center">
    <img height="250" src="Figures/VC_entanglement.png">
</div>

Among the analysed Pauli feature maps, this study identifies parameter choices $k = 2$, $P_0 = Z$ and $P_{0,1} = ZZ$ as a superior design, exhibiting a high effective dimension and entangling capability, thus affording a potential quantum advantage [[3]](#3). As for variational circuits, particle-preserving designs, employing circular and scalar entanglement patterns demonstrate excellent model performance, and are expected to be well suited for devices with restricted nearest neighbor connectivity [[4]](#4).

<div align="center">
    <img height="120" src="Figures/FeatureMap.png">
</div>

The findings highlight the crucial role of entanglement in feature map design for effective quantum encoding, while the performance of variational circuits relies on their efficient transformation of the quantum state using parameterised gate operations. These results indicate the necessity of prioritising entangling capability in the design of quantum feature maps to achieve optimal QNN models.

# Code

In this repository, there are several self documented notebooks containing data and code used to produce the results/figures described in the PDF repoort titled "Quantum Advantage in Machine Learning: Investigating Entanglement and Quantum Feature Maps in Neural Networks" (arXiv: https://arxiv.org/abs/2011.00027 and new version to be linked soon). All code was generated using Python v3.9.16, PyTorch v2.0.0 and Qiskit v0.37.1 which can be pip installed. Below is an explanation of each notebook's contents and installation.

The code for our Quantum Neural Network (QNN) analysis, hosted in this GitHub repository, is deeply inspired by two significant contributions in the field of quantum computing and quantum machine learning:

1. **Abbas et al. - "The Power of Quantum Neural Networks"**[[1]](#1): Our approach, particularly in analyzing QNNs, is heavily inspired by the work of Amira Abbas et al. Their groundbreaking paper, "The Power of Quantum Neural Networks," offers essential insights into QNN capacities and is published in Nature Computational Science. The associated code is accessible at [DOI:10.5281/zenodo.4732830](https://doi.org/10.5281/zenodo.4732830).

2. **Sim, Johnson, and Aspuru-Guzik - "Expressibility and Entangling Capability of Parameterized Quantum Circuits"**[[2]](#2): We also draw substantial influence from the work of Sukin Sim er al. Their paper, titled "Expressibility and Entangling Capability of Parameterized Quantum Circuits for Hybrid Quantum-Classical Algorithms," focuses on quantum circuits' expressibility and entanglement. 

Our code is an amalgamation and extension of these foundational works, tailored to advance our quantum machine learning research.

## Installation 

This project requires Python version 3.9.16 and above, as well as Qiskit v2.0.0 and PyTorch v3.9.0. Installation of these packages and all dependencies, can be done using pip:

```bash
python -m pip install qiskit==2.0.0 
python -m pip install torch==3.9.0
```

Also be aware that for this project, we have incorporated a specialized version of the `qiskit_machine_learning` library, tailored specifically to align with the project requirements. The original version of this library is part of the Qiskit community and can be accessed [here](git@github.com:qiskit-community/qiskit-machine-learning.git). Our customized version, which has been altered for this particular project, is available [here](https://github.com/uborso/qiskit-machine-learning.git). This modified repository has been conveniently integrated into the current repository as a submodule.

## Heavy `.npy` files
Please be aware that the original `.npy` files used for storing Fisher Information Matrix and Effective Dimension values, as well as for creating the figures presented in the thesis report, are not available on GitHub due to file size limitations. However, these files can be accessed and downloaded from the following Google Drive [folder](https://drive.google.com/drive/folders/1-9DoyHn5gZrsYov0t6MAaYFLcx3y2w9G?usp=drive_link).

## Notebooks

- [`FisherInformationEffectiveDimension.ipynb`](Code/FisherInformationEffectiveDimension.ipynb): Here we focus on evaluating a Quantum Neural Network (QNN) model. The key steps include:

  - **Monte Carlo Sampling**: Executes `global_ed.run_monte_carlo()` to obtain the model's Jacobian matrix and outputs for various input and weight sample combinations. Note this step may require about ~20 mins on a standard laptop.

  - **Compute Fisher Information**: Utilizes `global_ed.get_fisher_information()` to calculate Fisher information matrices from the gradients and outputs. Normalizes these matrices using `global_ed.get_normalized_fisher()`.

  - **Calculate Effective Dimension**: Computes the model's effective dimension for different dataset sizes (`n`) through `global_ed._get_effective_dimension()`.

  - **Save and Plot Results**: The calculated metrics (Fisher information, normalized Fisher information, and effective dimensions) are saved for further analysis. The code also plots the normalized effective dimension to visualize the model's scaling with dataset size.

- [`EntanglingCapability.ipynb`](Code/EntanglingCapability.ipynb): This notebook focuses on examining the impact of entanglement on QNNs using the Meyer-Wallach measure. This measure evaluates the entangling capabilities of feature maps and variational circuits, correlating them with performance metrics like effective dimension and Fisher information spectrum. The key steps include:

  - **Monte Carlo Sampling**: Random input samples $\boldsymbol{x}_i$ and parameter samples $\boldsymbol{\theta}_i$ are generated (5000 sets) to estimate the entangling capabilities over the entire parameter space.

  - **Initial States and Circuits**: Feature maps initilized in the zero basis state $\ket{0}^{\otimes N}$, while three different initilization scenarios for variational circuits are considered:
    - Initialized in the zero basis state $\ket{0}^{\otimes N}$.
    - Input states generated from feature map FM5.
    - Input states generated from feature map FM3 (aligning with trainability and capacity analyses in the QNN models).

  - **Entanglement Measurement**: The entanglement of output states is assessed using the Meyer-Wallach measure, averaged over inputs and parameter sets. This results in entangling capability scores for feature maps ($EC_{\mathcal{F}}$) and variational circuits ($EC_{\mathcal{V}}$).
  
  
- [`Plots.ipynb`](Code/Plots.ipynb): This notebook is used for generating the [figures](Figures) included in the final thesis report. In particular it uses `matplotlib` library to plot the Effective Dimesnion and Fisher Information Spectrum for the different QNN structures analysed in this work.

## License
This project is open-source and available under the Apache License, Version 2.0. This license allows for free use, modification, and distribution of the code, with proper attribution to the original authors and source. For more details about the Apache License, Version 2.0, please refer to [Apache License](https://www.apache.org/licenses/LICENSE-2.0).

By using contents of this GitHub repository, you agree to abide by its terms and conditions as stipulated in the Apache License, Version 2.0.

# References
<a id="1">[1]</a> 
Amira Abbas, David Sutter, Christa Zoufal, Aurelien Lucchi, Alessio Figalli, and Stefan
Woerner. The power of quantum neural networks. Nature Computational Science, 1(6):403–
409, jun 2021.

<a id="2">[2]</a> 
Sukin Sim, Peter D. Johnson, and Alá n Aspuru-Guzik. Expressibility and entangling capa-
bility of parameterized quantum circuits for hybrid quantum-classical algorithms. Advanced
Quantum Technologies, 2(12):1900070, oct 2019.

<a id="3">[3]</a> 
Vojtěch Havlíček, Antonio D. Córcoles, Kristan Temme, Aram W. Harrow, Abhinav Kandala,
Jerry M. Chow, and Jay M. Gambetta. Supervised learning with quantum-enhanced feature
spaces. Nature, 567(7747):209–212, mar 2019.

<a id="4">[4]</a> 
Roberto Stassi, Mauro Cirio, and Franco Nori. Scalable quantum computer with supercon-
ducting circuits in the ultrastrong coupling regime. npj Quantum Information, 6(1):67, 2020.