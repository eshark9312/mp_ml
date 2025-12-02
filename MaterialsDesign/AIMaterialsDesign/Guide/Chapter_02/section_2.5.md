# Section 2.5: Basis Sets and Computational Methods

## Representing Wavefunctions Numerically

To solve the Kohn-Sham equations on a computer, we need to represent the wavefunctions (orbitals) in a numerical form. We can't work with continuous functions directly—we must expand them in a basis set, which is a set of known functions that we can combine to approximate the true wavefunction. This expansion converts the problem of finding a continuous function into the problem of finding a finite set of coefficients, which is what computers can handle.

The choice of basis set is crucial because it determines both the accuracy we can achieve and the computational cost. A good basis set should be:
- **Complete**: Able to represent any wavefunction to arbitrary accuracy (given enough basis functions). This means that as we add more basis functions, we can get arbitrarily close to the true wavefunction.
- **Efficient**: Requiring as few basis functions as possible for a given accuracy. Some basis sets can represent wavefunctions accurately with fewer functions than others, which reduces computational cost.
- **Well-suited**: Appropriate for the type of system (molecules vs. crystals). Different systems have different symmetries and properties, and basis sets that respect these properties are more efficient.

Different types of systems require different basis sets. For periodic systems (crystals), plane waves are natural because they respect the periodicity of the crystal. For isolated molecules, atomic orbitals are often more efficient because they're localized around atoms, matching the physical structure of molecules. The choice of basis set is one of the first decisions in a DFT calculation, and it significantly affects both accuracy and computational cost.

The basis set expansion converts the continuous wavefunction into a discrete representation. Instead of working with $\psi(\mathbf{r})$ at every point in space, we work with a finite set of coefficients that determine how basis functions are combined. This discretization is what makes the problem computationally tractable, but it also introduces approximations. The quality of these approximations depends on the basis set chosen and how many basis functions are included.

## Plane Wave Basis Sets

For periodic systems like crystals, plane waves are the natural choice. A plane wave is a function that oscillates in space with a specific wavelength and direction. Mathematically, a plane wave is:

$$e^{i\mathbf{G}\cdot\mathbf{r}}$$

where $\mathbf{G}$ is a wave vector (reciprocal lattice vector) that determines the wavelength and direction of oscillation.

Any periodic function can be expanded as a sum of plane waves:

$$\psi_i(\mathbf{r}) = \sum_{\mathbf{G}}c_{i,\mathbf{G}}e^{i\mathbf{G}\cdot\mathbf{r}}$$

The coefficients $c_{i,\mathbf{G}}$ are determined by solving the Kohn-Sham equations. The sum is over all reciprocal lattice vectors $\mathbf{G}$ up to some maximum energy cutoff.

### The Energy Cutoff

The key parameter in a plane wave calculation is the energy cutoff $E_{cut}$. This determines the maximum kinetic energy of the plane waves included in the basis set:

$$E_{cut} = \frac{\hbar^2|\mathbf{G}_{max}|^2}{2m_e}$$

Higher cutoff means more plane waves and higher accuracy, but also higher computational cost. The number of plane waves scales as $E_{cut}^{3/2}$, so doubling the cutoff increases the number of basis functions by about 2.8 times.

### Advantages of Plane Waves

Plane waves have several advantages that make them ideal for periodic systems:

**Systematic improvement**: You can always improve accuracy by increasing the cutoff. There's a clear path to convergence.

**Efficient for periodic systems**: Plane waves naturally respect the periodicity of crystals, making them very efficient.

**Easy to implement**: The mathematics is straightforward, and many efficient algorithms exist.

**Good for metals**: Plane waves handle the Fermi surface of metals well, which can be challenging for other basis sets.

### Disadvantages of Plane Waves

However, plane waves also have limitations:

**Requires pseudopotentials**: Core electrons are tightly bound and require very high energy plane waves to describe. This makes all-electron calculations with plane waves impractical. Pseudopotentials solve this problem (see below).

**Not efficient for molecules**: For isolated molecules, plane waves require a large "supercell" to avoid interactions between periodic images, making calculations inefficient.

**Many basis functions needed**: Even with pseudopotentials, plane wave calculations typically require thousands of basis functions per atom.

## Pseudopotentials

Core electrons (those in filled shells close to the nucleus) are tightly bound and don't participate in chemical bonding. However, they require very high energy plane waves to describe accurately. Pseudopotentials solve this problem by replacing the strong nuclear potential and core electrons with a smoother "pseudopotential" that reproduces the behavior of valence electrons.

### The Pseudopotential Concept

The idea is to create an effective potential that, when used in a calculation, gives the same results for valence electrons as the full all-electron calculation would. The pseudopotential is smoother than the true potential, so it requires fewer plane waves.

### Types of Pseudopotentials

**Norm-conserving pseudopotentials**: These preserve the charge density outside the core region. They're accurate but require relatively high cutoffs.

**Ultrasoft pseudopotentials**: These relax the norm-conserving condition, allowing smoother potentials and lower cutoffs. They require special treatment in the calculation but are more efficient.

**PAW (Projector Augmented Wave)**: This method uses a transformation between smooth pseudo wavefunctions and all-electron wavefunctions. It's more accurate than ultrasoft pseudopotentials while maintaining efficiency.

PAW is widely used in modern DFT codes like VASP and GPAW because it provides a good balance between accuracy and efficiency.

## Atomic Orbital Basis Sets

For molecules and clusters, atomic orbitals are often more efficient than plane waves. Atomic orbitals are functions centered on atoms that resemble the electron distributions in isolated atoms.

### Gaussian-Type Orbitals (GTOs)

In quantum chemistry, Gaussian-type orbitals are very popular:

$$g(\mathbf{r}) = N e^{-\alpha|\mathbf{r}-\mathbf{R}|^2}$$

where $N$ is a normalization constant, $\alpha$ determines the width, and $\mathbf{R}$ is the atomic position.

GTOs are used because:
- Products of Gaussians are easy to integrate analytically
- They can be combined to approximate atomic orbitals
- Efficient algorithms exist for molecular calculations

### Numerical Atomic Orbitals

Some codes use numerical atomic orbitals, which are computed numerically for each atom type. These can be more accurate than GTOs but are less flexible.

## Convergence and Accuracy

Regardless of the basis set chosen, it's essential to test convergence. This means checking that results don't change significantly when you increase the basis set size (increase cutoff for plane waves, or add more functions for atomic orbitals).

### Key Convergence Parameters

**Energy cutoff** (plane waves): Increase until total energy and forces converge.

**k-point sampling** (periodic systems): Increase the density of k-points in the Brillouin zone until properties converge.

**Basis set size** (atomic orbitals): Add more functions until results converge.

**SCF convergence**: The self-consistent field iteration must converge to a stable solution.

Proper convergence testing is essential for reliable results. A calculation that hasn't converged can give misleading results, and these errors can propagate to AI models trained on the data.

---

**Previous Section**: [Section 2.4: Exchange-Correlation Functionals](section_2.4.md)  
**Next Section**: [Section 2.6: Typical Workflows and Best Practices](section_2.6.md)

