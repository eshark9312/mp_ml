# Section 2.6: Typical Workflows and Best Practices

## The DFT Calculation Workflow

A typical DFT calculation follows a well-established workflow, from initial structure to final properties. Understanding this workflow is essential for generating high-quality data for AI models. Each step requires careful attention to ensure accurate and reliable results. The workflow is not just a sequence of steps—it's a process that requires making many decisions, each of which affects the final results.

The workflow begins with preparing the initial structure and ends with extracting desired properties. In between, there are many steps: choosing computational parameters, running structure optimization, checking convergence, and computing properties. Each step has potential pitfalls, and understanding these pitfalls is crucial for generating reliable data. Poor workflow design or execution can lead to inaccurate results, which will contaminate any AI models trained on the data.

The workflow also requires balancing multiple considerations: accuracy, computational cost, and time. More accurate calculations take longer and cost more, so there's often a trade-off. For high-throughput calculations where many materials need to be computed, faster but less accurate settings might be appropriate. For final verification of promising candidates, more accurate settings are needed. Understanding these trade-offs helps in designing effective workflows.

Additionally, the workflow must be reproducible. Others (or you in the future) should be able to reproduce your calculations exactly. This requires documenting all parameters, software versions, and procedures. Reproducibility is essential for scientific validity and for building reliable datasets that can be used by the community.

## Structure Optimization

The first step in most DFT calculations is structure optimization—finding the equilibrium atomic positions and, for crystals, the equilibrium lattice parameters. This is crucial because material properties depend strongly on structure.

### Initial Structure

The starting point can come from several sources:
- **Experimental structures**: From X-ray or neutron diffraction experiments, available in databases like ICSD (Inorganic Crystal Structure Database) or COD (Crystallography Open Database)
- **Database structures**: From computational databases like the Materials Project
- **Structure prediction**: From evolutionary algorithms or other prediction methods
- **Prototype structures**: Common crystal structure types (perovskite, spinel, etc.)

The quality of the initial structure matters. A good initial guess can lead to faster convergence and the correct final structure. A poor initial guess might converge to a local minimum (a metastable structure) rather than the global minimum (the most stable structure).

### Geometry Relaxation

Geometry relaxation involves minimizing the total energy with respect to atomic positions. The algorithm typically:
1. Calculates forces on each atom (the negative gradient of energy)
2. Moves atoms in the direction that reduces energy
3. Repeats until forces are below a threshold (typically 0.01-0.05 eV/Å)

For crystals, we also optimize the unit cell parameters (lattice constants and angles). This requires calculating the stress tensor and minimizing it.

### Convergence Criteria

Proper convergence is essential. The structure should be relaxed until:
- **Forces**: All atomic forces are below a threshold (typically 0.01 eV/Å for accurate calculations)
- **Stress**: For crystals, the stress tensor should be close to zero
- **Energy**: The energy should not change significantly between iterations

Under-converged structures can lead to incorrect properties, which will contaminate any AI models trained on the data.

## Property Calculation

Once the structure is optimized, we can calculate various properties. The choice of properties depends on the application, but common ones include:

### Electronic Structure Properties

**Band structure**: The energy of electronic states as a function of wave vector. This reveals whether a material is a metal, semiconductor, or insulator, and provides information about electronic transport.

**Density of states (DOS)**: The number of electronic states at each energy. This helps understand electronic properties and can reveal features like band gaps, van Hove singularities, and the character of electronic states.

**Band gap**: The energy difference between the valence band maximum and conduction band minimum. Critical for semiconductors and optoelectronic applications.

### Mechanical Properties

**Elastic constants**: Describe how a material responds to stress. These are fundamental mechanical properties that determine stiffness, Poisson's ratio, and mechanical stability.

**Bulk modulus**: Resistance to uniform compression. Important for high-pressure applications.

**Shear modulus**: Resistance to shear deformation. Important for mechanical applications.

**Young's modulus**: Ratio of stress to strain in uniaxial deformation. A key engineering property.

### Thermodynamic Properties

**Formation energy**: Energy required to form a compound from its elements. Determines thermodynamic stability.

**Phase stability**: Which crystal structure is most stable at given conditions.

**Cohesive energy**: Energy required to separate a solid into isolated atoms.

### Optical Properties

**Dielectric function**: How a material responds to electric fields. Determines optical properties like refractive index and absorption.

**Optical absorption**: How light is absorbed, important for solar cells and optoelectronics.

### Magnetic Properties

**Magnetic moments**: Local and total magnetic moments of atoms.

**Exchange interactions**: How magnetic moments interact, determining magnetic ordering.

## Convergence Testing

Before using DFT results for AI training, it's essential to ensure calculations are properly converged. This means testing that results don't change when you increase computational parameters.

### Energy Cutoff Convergence

For plane wave calculations, test convergence of the energy cutoff:
1. Start with a low cutoff (e.g., 300 eV)
2. Increase gradually (400, 500, 600 eV...)
3. Plot total energy vs. cutoff
4. Choose cutoff where energy is converged (typically within 1 meV/atom)

### k-Point Convergence

For periodic systems, test k-point sampling:
1. Start with a coarse k-point mesh (e.g., 2×2×2)
2. Increase density (4×4×4, 6×6×6, 8×8×8...)
3. Check convergence of total energy and properties
4. Use a mesh that gives converged results

### SCF Convergence

The self-consistent field iteration must converge:
- Check that energy and density change by less than the convergence threshold
- Typical thresholds: 10⁻⁶ to 10⁻⁸ eV for energy, 10⁻⁶ for density
- If convergence fails, may need better initial guess or different mixing scheme

### Force Convergence

For geometry optimization:
- Forces should be below threshold (typically 0.01 eV/Å)
- Check that structure doesn't change significantly in final iterations
- Verify stress is near zero for equilibrium structures

## Best Practices for AI-Ready Data

When generating data for AI models, several best practices ensure high quality:

### Consistency

Use consistent calculation parameters across all materials:
- Same functional (e.g., PBE for all)
- Same basis set settings (same cutoff, same k-point density)
- Same convergence criteria

Inconsistencies can introduce systematic errors that confuse AI models.

### Documentation

Document all calculation parameters:
- Functional used
- Basis set (cutoff, k-points)
- Convergence criteria
- Software version
- Any special settings (DFT+U, vdW corrections, etc.)

This metadata is crucial for understanding and reproducing results.

### Quality Control

Check results for reasonableness:
- Formation energies should be negative for stable compounds
- Band gaps should be non-negative
- Structures should be physically reasonable
- Check for known systematic errors (e.g., band gap underestimation)

### Error Handling

Handle failed calculations appropriately:
- Don't include unconverged calculations in training data
- Document why calculations failed
- Consider retrying with different parameters

Failed calculations can introduce noise and errors into AI models.

---

**Previous Section**: [Section 2.5: Basis Sets and Computational Methods](section_2.5.md)  
**Next Section**: [Section 2.7: Software Packages and Tools](section_2.7.md)

