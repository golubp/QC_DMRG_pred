# QC_DMRG_pred
This repository contains the test and train data together with trained model from article "Quantum Chemical Density Matrix Renormalization Group Method Boosted by Machine Learning"

The module is aimed to predict static correlation energy for polycyclic aromatic hydrocarbons based
on low-bond DMRG calculations and correlation information.
See /url/of/the/article for more details.

To get prediction run:

        ./run_gnn_prediction dmrg_file

where dmrg_file is file containing all necessary information on low-bond DMRG calculation.
dmrg_file can be either in palin format or in fromat of MOLMPS output file.

PLAIN FORMAT

This is text file with the following content (assuming there are N orbitals in the system):

        1 line          -       float: Hartree-Fock energy, Ha
        2 line          -       float: low bond DMRG energy, Ha
        3 line          -       float: truncation error
        4 line          -       integers: occupation numbers for each orbital
        5 line
        ---             -       integer, float: orbital index (counting from 1), the corresponding single-site entropy
        (5+N) line
        (3+N) line
        ---             -       integer, integer, float: first orbital index, second orbital index, the corresponding two-site entropy
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
        occupations     -       1D array, [norbs ints], occupation numbers for each orbital
        entrop1         -       1D array, [norbs float], single-site entropies for each orbital
        entrop2         -       2d array, [norbs [norbs float]], two-site entropies for each orbital-orbtial pair

To get prediction in this case run:

        ./run_gnn_prediction dmrg_file any_name_you_wish
