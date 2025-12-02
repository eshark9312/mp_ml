# Section 3.3: Dataset Curation and Quality Control

## The Critical Importance of Data Quality

In machine learning, the adage "garbage in, garbage out" is particularly relevant. No matter how sophisticated your machine learning algorithms are, they cannot overcome poor-quality training data. This is because machine learning models learn patterns from data—if the data contains errors, inconsistencies, or biases, the models will learn those problems along with the physics. The result is models that make poor predictions, even if the algorithms themselves are sound.

For materials science applications, where we're dealing with computational data from first-principles calculations, ensuring data quality is a multi-faceted challenge that requires careful attention at every stage. The data generation process involves many steps—structure preparation, calculation setup, execution, result extraction, and validation. Problems can arise at any stage, and these problems can propagate through the pipeline, contaminating the final dataset. Systematic quality control is essential to catch and address these problems before they affect model training.

Data quality issues can arise from many sources: failed calculations, convergence problems, inconsistent computational settings, duplicate entries, or unphysical results. Each of these issues, if not addressed, can degrade model performance or lead to incorrect predictions. For example, failed calculations that aren't filtered out can create gaps in the dataset or introduce noise. Inconsistent settings can create systematic variations that confuse models. Duplicate entries can bias training toward certain materials. Unphysical results can lead to models that learn incorrect relationships. The goal of dataset curation is to identify and address these issues systematically, ensuring that the final dataset is clean, consistent, and reliable.

## Common Data Quality Issues

Understanding the types of problems that can occur in computational datasets helps in designing effective quality control procedures:

### Failed Calculations

Not all DFT calculations succeed. A calculation might fail for various reasons:
- **Convergence failures**: The self-consistent field iteration might not converge, or geometry optimization might not reach equilibrium
- **Resource limitations**: Calculations might exceed time or memory limits
- **Software errors**: Bugs or numerical instabilities can cause crashes
- **Input errors**: Incorrect input parameters can lead to failures

Failed calculations should be identified and excluded from training data, as they don't provide reliable information.

### Incomplete Data

Even when calculations complete, they might not have all desired properties. This can happen if:
- Some properties weren't calculated (to save computational time)
- Property calculations failed even though structure optimization succeeded
- Properties require additional calculations that weren't performed

Incomplete data creates challenges for training models that need multiple properties or for ensuring consistent datasets.

### Inconsistent Settings

One of the most insidious problems is inconsistency in computational settings. If different materials were calculated with different functionals, different energy cutoffs, or different convergence criteria, the dataset will have systematic variations that can confuse machine learning models.

For example, if half your data uses PBE functional and half uses LDA, the models will see systematic differences that aren't related to the materials themselves but to the computational method. This can lead to poor generalization and unreliable predictions.

### Duplicate Structures

The same material might appear multiple times in a dataset, calculated at different times or with different settings. Duplicates waste computational resources and can bias training data if they're not identified and removed.

### Unphysical Results

Sometimes calculations produce results that are physically unreasonable:
- Negative band gaps (which are unphysical—band gaps must be non-negative)
- Extremely negative formation energies (suggesting the material is impossibly stable)
- Negative volumes or other unphysical structural parameters
- Imaginary phonon frequencies (indicating structural instability)

These unphysical results often indicate calculation problems and should be flagged or removed.

## Quality Control Pipeline

A systematic quality control pipeline should check for all these issues and filter the data appropriately. The pipeline typically consists of several stages:

### Stage 1: Filter Failed Calculations

The first step is identifying and removing calculations that didn't complete successfully. This involves checking:
- Whether the calculation finished (not just started)
- Whether convergence was achieved
- Whether final energies and structures are available

Only calculations that completed successfully and converged should proceed to the next stage.

### Stage 2: Convergence Verification

Even calculations that "completed" might not have converged properly. It's important to verify:
- **Force convergence**: All atomic forces should be below a threshold (typically 0.01-0.05 eV/Å)
- **SCF convergence**: The self-consistent field iteration should have converged
- **Stress convergence**: For crystals, the stress tensor should be near zero for equilibrium structures
- **Energy convergence**: The energy should not be changing significantly in final iterations

Calculations that haven't converged properly may have inaccurate properties and should be flagged or excluded.

### Stage 3: Duplicate Removal

Duplicate structures should be identified and removed. This requires structure matching algorithms that can identify when two structures represent the same material, even if they're in different unit cells or have slightly different atomic positions.

Structure matching is non-trivial because the same material can be represented in different ways (different unit cells, different coordinate systems, etc.). Specialized algorithms are needed to identify true duplicates.

### Stage 4: Physical Validation

Results should be checked for physical reasonableness:
- **Formation energies**: Should be in a reasonable range (not impossibly negative or positive)
- **Band gaps**: Must be non-negative
- **Volumes**: Must be positive
- **Structural parameters**: Bond lengths, angles, etc. should be physically reasonable
- **Phonon frequencies**: Should be real (not imaginary) for stable structures

Unphysical results often indicate calculation problems and should be investigated or removed.

## Data Standardization

Ensuring consistency across datasets requires standardization:

### Unit Standardization

All quantities should use consistent units throughout the dataset:
- Energies in electronvolts (eV)
- Distances in Angstroms (Å)
- Forces in eV/Å
- Pressures in GPa or eV/Å³

Inconsistent units can cause serious problems for machine learning models, which are sensitive to the scale of input features.

### Reference State Consistency

Formation energies depend on the reference states chosen for elements. All calculations should use the same reference states (typically the stable crystal structure of each element at standard conditions).

If different reference states are used, formation energies will be inconsistent, and models trained on this data will be unreliable.

### Structure Format Standardization

Structures should be stored in a consistent format. While different formats (CIF, POSCAR, etc.) can be converted, it's important to ensure that the conversion preserves all information and that the same representation is used throughout.

### Property Definition Consistency

Properties must be defined consistently. For example, band gaps might be calculated as direct gaps, indirect gaps, or fundamental gaps. The definition used should be consistent and documented.

## Handling Missing Data

Real datasets often have missing values—some properties might not be available for all materials. Several strategies can be used:

### Removal Strategy

If a critical property is missing, the simplest approach is to remove that entry from the dataset. This is appropriate when:
- The missing property is essential for the application
- Only a small fraction of entries have missing values
- The missing values are random (not systematic)

### Imputation Strategy

Missing values can be estimated (imputed) using various methods:
- Mean or median imputation (replace with average value)
- Regression imputation (predict from other properties)
- Machine learning imputation (use models to predict missing values)

However, imputation should be used with caution, as it introduces uncertainty and can bias results if not done carefully.

### Flagging Strategy

Missing values can be explicitly flagged and handled by the machine learning model. Some models can handle missing values directly, or special features can be added to indicate missingness.

### Separate Models Strategy

If different subsets of materials have different properties available, separate models can be trained for each subset. This avoids the need for imputation but requires managing multiple models.

## Metadata and Provenance

High-quality datasets include comprehensive metadata that documents:
- **Calculation parameters**: Functional, basis set, convergence criteria
- **Software information**: Code version, compilation options
- **Computational resources**: Number of processors, memory, wall time
- **Date and time**: When calculations were performed
- **Provenance**: What calculations this one depends on (e.g., structure from previous relaxation)

This metadata is essential for:
- **Reproducibility**: Understanding exactly how results were obtained
- **Debugging**: Identifying problems when they occur
- **Analysis**: Understanding systematic effects and biases
- **Quality assessment**: Evaluating data quality and reliability

## The Impact on Machine Learning

The quality of the training data directly impacts machine learning model performance. Poor-quality data leads to:
- **Reduced accuracy**: Models can't learn correct patterns from noisy or incorrect data
- **Poor generalization**: Models trained on inconsistent data won't generalize well
- **Systematic biases**: Errors in training data propagate to model predictions
- **Unreliable predictions**: Models may give confident but incorrect predictions

Conversely, high-quality, well-curated data enables:
- **Better model performance**: Models can learn true underlying relationships
- **Reliable predictions**: Predictions can be trusted for decision-making
- **Generalization**: Models work well on new, unseen materials
- **Interpretability**: Models trained on good data provide meaningful insights

Investing time and effort in data curation pays dividends in model performance and reliability.

---

**Previous Section**: [Section 3.2: High-Throughput Computational Workflows](section_3.2.md)  
**Next Section**: [Section 3.4: Feature Extraction and Descriptors](section_3.4.md)

