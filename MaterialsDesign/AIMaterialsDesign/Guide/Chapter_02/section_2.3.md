# Section 2.3: Density Functional Theory (DFT) Basics

## The Revolutionary Insight

Density Functional Theory represents one of the most important theoretical developments in computational physics. The key insight, which earned Walter Kohn the Nobel Prize in Chemistry in 1998, is that we don't need to know the full many-electron wavefunction to calculate the ground-state energy and properties of a system. Instead, we only need to know the electron density—a function of just three spatial coordinates, regardless of how many electrons are in the system. This insight is revolutionary because it transforms an exponentially complex problem into a manageable one.

This is a remarkable simplification that fundamentally changes how we approach electronic structure calculations. Instead of working with a wavefunction that depends on the coordinates of all $N$ electrons (a $3N$-dimensional problem), we can work with the electron density $n(\mathbf{r})$, which is a function of just three spatial coordinates. For a system with 100 electrons, this reduces the problem from 300 dimensions to just 3 dimensions—a reduction by a factor of 100! This dimensional reduction is what makes DFT practical for materials calculations.

The electron density $n(\mathbf{r})$ tells us how many electrons are in a small volume around each point in space. It's a much simpler object than the full wavefunction—it's just a number at each point in space, rather than a complex function of all electron coordinates. Yet, remarkably, the Hohenberg-Kohn theorems (discussed below) prove that this simple density contains all the information needed to determine the ground-state energy and properties of the system.

This insight has profound implications. It means that we can solve the many-electron problem by finding the electron density that minimizes the energy, rather than finding the full many-electron wavefunction. This is still a challenging optimization problem, but it's vastly simpler than the original problem. The development of DFT has made it possible to compute properties for materials with hundreds or even thousands of atoms, enabling computational materials science as we know it today.

## The Hohenberg-Kohn Theorems

The theoretical foundation of DFT rests on two fundamental theorems proved by Pierre Hohenberg and Walter Kohn in 1964. These theorems are among the most important results in theoretical physics, providing the mathematical justification for the entire DFT approach.

### Theorem 1: The Uniqueness Theorem

The first Hohenberg-Kohn theorem states that the ground-state electron density $n(\mathbf{r})$ uniquely determines the external potential $v_{ext}(\mathbf{r})$ (and thus the Hamiltonian), up to an additive constant.

This theorem is profound because it tells us that the electron density contains all the information we need. If we know the electron density, we can uniquely determine what system we're dealing with. The external potential (which comes from the nuclei) is uniquely determined by the density, which means the entire Hamiltonian is determined, which means all properties are determined.

This is a one-to-one mapping: each unique electron density corresponds to a unique external potential, and vice versa. This means that instead of working with the wavefunction, we can work directly with the density, which is much simpler.

### Theorem 2: The Variational Principle

The second Hohenberg-Kohn theorem states that there exists a universal functional $F[n]$ such that the ground-state energy can be written as:

$$E[n] = \int v_{ext}(\mathbf{r})n(\mathbf{r})d\mathbf{r} + F[n]$$

and the ground-state density minimizes this functional.

The term "universal" here means that the functional $F[n]$ is the same for all systems—it doesn't depend on what atoms are present or how they're arranged. It's a property of the electron-electron interaction itself, not of any particular material.

The variational principle tells us that if we can find the density that minimizes this energy functional, we've found the ground state. This provides a practical route to solving the problem: we can search through different densities to find the one that minimizes the energy.

## The Kohn-Sham Equations

While the Hohenberg-Kohn theorems tell us that DFT is possible in principle, they don't tell us how to actually do it in practice. The challenge is that the universal functional $F[n]$ is unknown. We know it exists, but we don't know its exact form.

Walter Kohn and Lu Jeu Sham solved this problem in 1965 by introducing a brilliant trick: they proposed using a reference system of non-interacting electrons that has the same density as the real, interacting system. This reference system is much easier to solve because non-interacting electrons don't repel each other.

The Kohn-Sham equations are a set of single-electron equations:

$$\left[-\frac{\hbar^2}{2m_e}\nabla^2 + v_{eff}(\mathbf{r})\right]\psi_i(\mathbf{r}) = \epsilon_i\psi_i(\mathbf{r})$$

These look like the Schrödinger equation for a single electron, but the effective potential $v_{eff}(\mathbf{r})$ is chosen such that the density of this non-interacting system matches the density of the real, interacting system.

The effective potential has three components:

$$v_{eff}(\mathbf{r}) = v_{ext}(\mathbf{r}) + v_{Hartree}(\mathbf{r}) + v_{xc}(\mathbf{r})$$

**External potential** $v_{ext}(\mathbf{r})$: This comes from the nuclei and represents the attraction between electrons and nuclei. It's the same as in the original problem.

**Hartree potential** $v_{Hartree}(\mathbf{r})$: This represents the classical electron-electron repulsion. It's calculated as if the electrons were classical charged particles, ignoring their quantum nature. This is the "mean-field" approximation—each electron sees the average repulsion from all other electrons.

**Exchange-correlation potential** $v_{xc}(\mathbf{r})$: This is the crucial term that captures all the quantum mechanical effects that the Hartree term misses. It includes both exchange (the quantum mechanical effect that prevents two electrons with the same spin from being in the same place) and correlation (the effect of electrons avoiding each other due to their mutual repulsion).

The electron density is constructed from the Kohn-Sham orbitals:

$$n(\mathbf{r}) = \sum_{i=1}^{N}|\psi_i(\mathbf{r})|^2$$

This is simply the sum of the probability densities of all occupied orbitals.

## The Self-Consistent Field (SCF) Procedure

The Kohn-Sham equations present a chicken-and-egg problem: to calculate the effective potential, we need to know the electron density, but to calculate the electron density, we need to solve the Kohn-Sham equations, which depend on the effective potential.

The solution is an iterative procedure called the Self-Consistent Field (SCF) method:

1. **Initial guess**: We start with an initial guess for the electron density $n(\mathbf{r})$. This might come from atomic densities, a previous calculation, or a simple approximation.

2. **Construct potential**: Using this density, we build the effective potential $v_{eff}(\mathbf{r})$. This involves calculating the Hartree potential (which requires integrating over the density) and the exchange-correlation potential (which depends on the density and possibly its gradients).

3. **Solve Kohn-Sham equations**: We solve the Kohn-Sham equations to find the orbitals $\psi_i(\mathbf{r})$ and their energies $\epsilon_i$. This is typically done by expanding the orbitals in a basis set and solving a matrix eigenvalue problem.

4. **Update density**: From the new orbitals, we calculate a new electron density using the formula above.

5. **Check convergence**: We compare the new density with the old density. If they're the same (within some tolerance), we've achieved self-consistency and we're done. If not, we go back to step 2 and repeat.

This iterative process continues until self-consistency is achieved—when the density used to construct the potential is the same as the density that comes out of solving the equations. Typically, this requires 10-50 iterations, depending on the system and the quality of the initial guess.

## The Exchange-Correlation Functional

The key challenge in DFT is that the exchange-correlation functional $E_{xc}[n]$ is unknown and must be approximated. This is where the art and science of DFT meet: different approximations to the exchange-correlation functional lead to different levels of accuracy for different properties.

The exchange-correlation energy is typically a small fraction of the total energy (often 1-5%), but it's crucial for getting accurate results. Small errors in the exchange-correlation functional can lead to significant errors in predicted properties. This is why the development of better exchange-correlation functionals has been a major research area for decades.

In the next section, we'll explore the various approximations that have been developed, from the simple Local Density Approximation to more sophisticated hybrid functionals, and discuss when to use each one.

---

**Previous Section**: [Section 2.2: The Quantum Mechanical Problem](section_2.2.md)  
**Next Section**: [Section 2.4: Exchange-Correlation Functionals](section_2.4.md)

