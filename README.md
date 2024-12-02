## Final Project: Phys 25000 Computational Physics
Final Project for Phys 25000. Restricted Hartree-Fock SCF ground state energy calculation, and plotting PECs. 

### Theoretical Overview of Hartree-Fock Self-Consistent Field
HF-SCF is a method for variationally optimizing a Slater-Determinant wavefunction by solving for molecular orbital coefficients self-consistently (iteratively). The following is a somewhat high-level overview of what this code will seek to accomplish, without getting too much into the weeds of the mathematical derivations. 

The Schrodinger equation can be written rather elegantly as 

$$ E = \bra{\Psi}H\ket{\Psi}$$

where E represents the energy eigenvalue associated with a particle's wavefunction $\Psi$ and H is the Hamiltonian operator. Using Hartree-Fock theory, we can 'solve' the Schrodinger equation by starting with a trial wavefunction $\Psi_{i}$, which is used to guess $\Psi_{i+1}$, and so on iteratively to minimize E (this is known as the variational method or variational principle). Since the wavefunctions of different orbitals themselves will be orthogonal, we are in practice optimizing a matrix of molecular orbital coefficients. 

To do this, we employ the Roothan equations, which are 'pseudo-eigenvalue' matrix equations given by

$$\sum_{\nu} F_{\mu\nu}C_{\nu i} = \epsilon_i\sum_{\nu}S_{\mu\nu}C_{\nu i}$$
$${\bf FC} = {\bf SC\epsilon},$$

where C is an orbital coefficient matrix and the orbital energy eigenvalues are given by $\epsilon_i$. These calculations use a basis set of orthonormal wavefunctions that describe atomic orbitals (AOs). Different AO basis sets, which are derived from different starting point approximations to AO wavefunctions, may be used. For example, the STO-nG basis sets use n linear combinations of primitive Gaussian functions to approximate electron probability distributions in different orbitals. F is the Fock matrix, which has elements $F_{\mu\nu}$ given by 

$$F_{\mu\nu} = H_{\mu\nu} + (\mu\,\nu\left|\lambda\,\sigma)P_{\lambda\sigma} - 1/2(\mu\,\lambda\right|\nu\,\sigma)P_{\lambda\sigma},$$

where $P_{\lambda\sigma}$ is an element of the one-particle density matrix P, constructed from the orbital coefficient matrix C:

$$P_{\lambda\sigma} = 2 \sum_{i} C_{\sigma i}C_{\lambda i}$$

In physical terms:
- The orbital coefficient matrix describes every atom's contribution (from an orbital basis set) to each particular molecular orbital.
- The density matrix describes the electron density contained in each molecular orbital.
- The term following H in the Fock matrix expression is the exchange integral J, which is used to enforce antisymmetry.
- The last term in the Fock matrix expression is the overlap integral K, which is used to prevent 'double counting' when we account for exchange correlation in J. 

The electronic RHF energy is given by 

$$E_{elec}^{RHF} = 0.5(F_{\nu\mu} +H_{\nu\mu})P_{\nu\mu}$$

and the total RHF energy is given by

$$E^{RHF} = E_{elec}^{RHF} + E_{nuc}^{RHF}$$

where $E^{RHF}_{nuc}$ is the nuclear repulsion energy which we calculate separately using the Born-Oppenheimer approximation to the Schrodinger Equation.

(RHF, or Restricted Hartree-Fock, basically means that a physical constraint is enforced on the electron's orbital occupation based on their spins. It is usually used for closed-shell systems, systems that have an even number of electrons.)

The Fock matrix by itself can't be diagonalized to solve for the coefficient matrix C, since it contains $S_{\mu\nu}$, the overlap. So, lastly, you have to tranform these matrices using "symmetric orthogonalization" to solve them for $C{\prime}$:

$$ F{\prime}C{\prime} = C{\prime} \epsilon $$

then transform them back into the original atomic orbital basis. 

#### Credits
Material taken from MENG 26110, Intermediate Quantum Engineering II (Winter 2024) Course Notes, written by Dr. Laura Gagliardi and Dr. Ruhee D'Cunha. Sources for these notes include
1. A. Szabo and N. S. Ostlund, *Modern Quantum Chemistry*, Introduction to Advanced Electronic Structure Theory. Courier Corporation, 1996.
2. I. N. Levine, *Quantum Chemistry*. Prentice-Hall, New Jersey, 5th edition, 2000.
3. T. Helgaker, P. Jorgensen, and J. Olsen, *Molecular Electronic Structure Theory*, John Wiley & Sons Inc, 2000.

The primary packages used for the actual python RHF script are NumPy, Psi4, and MatPlotLib (which I just used for plotting results). Links to Documentation:
- Numpy: https://numpy.org/doc/stable/
- Psi4: https://psicode.org/psi4manual/master/index.html
- MatPlotLib: https://matplotlib.org/stable/users/index.html

Note: Psi4 was tricky to install through Anaconda. I had a much easier time using conda-forge with MiniForge to install Psi4 and its dependencies, and then building a conda environment in MiniForge's terminal to download and run Jupyter Notebook. 
