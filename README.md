# QC_DMRG_pred
This repository contains the test and train data together with trained models from articles "Quantum Chemical Density Matrix Renormalization Group Method Boosted by Machine Learning" and "Fast and Accurate Excitation Energies from Density Matrix Renormalization Group Calculations Improved by Machine Learning".

The data for the first article is in the folder **I.Stat_Corr_Energies**. The data for the second article is in the folder **II.S0_S1_Gaps**.

# I.Stat_Corr_Energies

The module is aimed to predict static correlation energy for polycyclic aromatic hydrocarbons based
on low-bond DMRG calculations and correlation information.
See https://doi.org/10.1021/acs.jpclett.5c00207 for more details.

To get prediction run:

        ./run_gnn_prediction dmrg_file

where dmrg_file is file containing all necessary information on low-bond DMRG calculation.
dmrg_file can be either in plain format or in format of MOLMPS (https://gitlab.com/molmps/scalable) output file.

PLAIN FORMAT

This is text file with the following content (assuming there are N orbitals in the system):

        1 line          -       float: Hartree-Fock energy, Ha
        2 line          -       float: low bond DMRG energy, Ha
        3 line          -       float: truncation error
        4 line          -       integers: occupation numbers for each orbital
        5 line
        ---             -       integer, float: orbital index (counting from 1), single-site entropy
        (5+N) line
        (3+N) line
        ---             -       integer, integer, float: first orbital index, second orbital index, two-site entropy
        EOF

See for examples in directory /test

YOUR OWN FILE FORMAT

It is possible to use your own file format, but in this case a function for reading the data must be provided.

Create a module any_name_you_wish.py. Define inside function

        def read_data_():

It must end with

                return norbs, TRE, energies, occupations, entrop1, entrop2

where

        norbs           -       int, number of orbitals
        TRE             -       float, truncation error
        energies        -       1D array, [2 floats], first: Hartree-Fock energy, Ha; second: low bond DMRG energy, Ha
        occupations     -       1D array, [norbs ints], occupation numbers for each orbital in ground state
        entrop1         -       1D array, [norbs float], single-site entropies for each orbital
        entrop2         -       2D array, [norbs [norbs float]], two-site entropies for each orbital-orbital pair

To get prediction in this case run:

        ./run_gnn_prediction dmrg_file any_name_you_wish

# II.S0_S1_Gaps

The module is aimed to predict S0-S1 transition energies corrected for static correlation for polycyclic aromatic hydrocarbons based
on low-bond DMRG calculations and correlation information.

To get prediction run:

        ./run_gnn_prediction dmrg_file

where dmrg_file is file containing all necessary information on low-bond DMRG calculation.
dmrg_file can be either in plain format or in format of MOLMPS (https://gitlab.com/molmps/scalable) output file.

PLAIN FORMAT

This is text file with the following content (assuming there are N orbitals in the system):

        1 line          -       float: Hartree-Fock energy, Ha
        2 line          -       float, float: ground state DMRG energy, excited state DMRG energy, Ha
        3 line          -       float: truncation error
        4 line          -       integers: occupation numbers for each orbital
        5 line
        ---             -       integer, float, float: orbital index (counting from 1), single-site entropy for the ground state, single-site entropy for the excited state
        (5+N) line
        (3+N) line
        ---             -       integer, integer, float, float: first orbital index, second orbital index, two-site entropy for the ground state, two-site entropy for the excited state
        EOF 

See for examples in directory /test

YOUR OWN FILE FORMAT

It is possible to use your own file format, but in this case a function for reading the data must be provided.

Create a module any_name_you_wish.py. Define inside function

        def read_data_():

It must end with

                return norbs, TRE, energies, occupations, entrop1, entrop2

where

        norbs           -       int, number of orbitals
        TRE             -       float, truncation error
        energies        -       1D array, [3 floats], first: Hartree-Fock energy, Ha; second: ground state DMRG energy, Ha; third: excited state DMRG energy, Ha
        occupations     -       1D array, [norbs ints], occupation numbers for each orbital in ground state
        entrop1         -       2D array, [2 [norbs float]], single-site entropies for each orbital; first dimension: in ground state; second dimension: in excited state
        entrop2         -       3D array, [2 [norbs [norbs float]]], two-site entropies for each orbital-orbital pair; first dimension: in ground state; second dimension: in excited state

To get prediction in this case run:

        ./run_gnn_prediction dmrg_file any_name_you_wish
