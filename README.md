## Final Project: Phys 25000 Computational Physics
Final Project for Phys 25000. Restricted Hartree-Fock SCF ground state energy calculation, and plotting PECs. 

### Theoretical Overview of Hartree-Fock Self-Consistent Field
HF-SCF is a method for variationally optimizing a Slater-Determinant wavefunction by solving for molecular orbital coefficients self-consistently (iteratively). The following is a somewhat high-level overview of what this code will seek to accomplish, without getting too much into the weeds of the mathematical derivations. 

The HF-SCF method seeks to solve the Roothan equations, which are 'pseudo-eigenvalue' matrix equations given by

$$\sum_{\nu} F_{\mu\nu}C_{\nu i} = \epsilon_i\sum_{\nu}S_{\mu\nu}C_{\nu i}$$
$${\bf FC} = {\bf SC\epsilon},$$

where C is an orbital coefficient matrix and the orbital energy eigenvalues are given by $\epsilon_i$. These calculations use a basis set of orthonormal wavefunctions that describe atomic orbitals (AOs). Different AO basis sets, which are derived from different starting point approximations to AO wavefunctions, may be used. For example, the STO-nG basis sets use n linear combinations of primitive Gaussian functions to approximate electron probability distributions in different orbitals. F is the Fock matrix, which has elements $F_{\mu\nu}$ given by 

$$F_{\mu\nu} = H_{\mu\nu} + (\mu\,\nu\left|\lambda\,\sigma)P_{\lambda\sigma} - 1/2(\mu\,\lambda\right|\nu\,\sigma)P_{\lambda\sigma},$$

where $P_{\lambda\sigma}$ is an element of the one-particle density matrix P, constructed from the orbital coefficient matrix C:

$$P_{\lambda\sigma} = 2 \sum_{i} C_{\sigma i}C_{\lambda i}$$

In physical terms:
- The orbital coefficient matrix describes every atom's contribution (from an orbital basis set) to each particular molecular orbital.
- The density matrix describes the electron density contained in each molecular orbital.

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

I also referenced LibreTexts Chemistry quite a bit: https://chem.libretexts.org/Bookshelves/Physical_and_Theoretical_Chemistry_Textbook_Maps/Book%3A_Quantum_States_of_Atoms_and_Molecules_(Zielinksi_et_al)/10%3A_Theories_of_Electronic_Molecular_Structure/10.08%3A_The_Self-Consistent_Field_and_the_Hartree-Fock_Limit 

