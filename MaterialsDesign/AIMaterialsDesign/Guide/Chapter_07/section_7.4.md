# Section 7.4: Data Repositories and Databases

## Accessing Materials Data

Large materials databases are essential resources for AI-accelerated materials discovery. They provide training data for machine learning models, reference data for validation, and starting points for new calculations. These databases represent years of computational effort and are invaluable for the materials science community. Without them, each researcher would need to generate their own training data, which would be prohibitively expensive and time-consuming.

The value of these databases extends beyond just providing data. They enable reproducibility—researchers can reference specific materials from databases, and others can access the same data. They enable comparison—researchers can compare their results to database values to validate their methods. They enable discovery—researchers can explore database contents to find interesting materials or patterns. And they enable collaboration—different groups can work with the same data, facilitating collaboration and comparison of results.

However, using databases effectively requires understanding their contents, limitations, and how to access them. Different databases have different strengths, different coverage, and different access methods. Understanding these differences helps in choosing the right database for your needs and using it effectively. Additionally, databases are living resources that evolve over time—new materials are added, properties are recomputed with better methods, and interfaces are improved. Staying current with database developments is important for making the most of these resources.

## The Materials Project

The Materials Project, founded at Lawrence Berkeley National Laboratory, is one of the largest and most widely used materials databases.

### Contents

The Materials Project contains:
- **Over 150,000 materials** with computed properties
- **Formation energies** for thermodynamic stability
- **Band structures and densities of states** for electronic properties
- **Elastic tensors** for mechanical properties
- **Crystal structures** in standardized formats

### Access

The Materials Project provides:
- **Web interface**: Browse and search materials through a user-friendly website
- **REST API**: Programmatic access for automated data retrieval
- **Python interface**: Through pymatgen, making it easy to access data in Python workflows

### Applications

The Materials Project is widely used for:
- Training machine learning models
- Finding starting structures for calculations
- Validating computational results
- Exploring materials space

The scale and quality of the Materials Project make it an invaluable resource for the computational materials science community.

## AFLOW

AFLOW (Automatic Flow for materials discovery) is another major materials database with over 3 million materials.

### Comprehensive Coverage

AFLOW provides:
- Extensive coverage of composition space
- Multiple properties computed consistently
- REST API for programmatic access
- Integration with computational workflows

### Strengths

AFLOW's large size makes it valuable for training models that need extensive data. Its systematic coverage of composition space helps ensure diverse training data.

## OQMD: Open Quantum Materials Database

The Open Quantum Materials Database (OQMD) contains over 1 million materials with computed properties.

### Focus

OQMD focuses on:
- Formation energies and thermodynamic stability
- Electronic structure properties
- Systematic exploration of composition space

### Open Access

As an open database, OQMD provides free access to all data, making it valuable for researchers without access to commercial databases.

## NOMAD: The European Materials Database

NOMAD (Novel Materials Discovery) is a European initiative that aggregates data from multiple sources.

### Aggregation

NOMAD collects data from:
- Multiple computational groups
- Different DFT codes
- Various computational settings

This aggregation provides a comprehensive view of available computational data.

### Standardization

NOMAD works to standardize data formats and metadata, making it easier to combine data from different sources.

## COD: Crystallography Open Database

The Crystallography Open Database (COD) contains experimental crystal structures from published literature.

### Experimental Data

Unlike computational databases, COD contains experimentally determined structures, providing:
- Ground truth structures
- Validation data for computational methods
- Starting points that are known to be synthesizable

### Scale

With over 400,000 experimental structures, COD is a valuable resource for both computational and experimental researchers.

## Using Databases Effectively

Effective use of materials databases requires:

**Understanding data quality**: Know how data was computed (functional, settings, etc.) to understand its limitations

**Filtering appropriately**: Use database query capabilities to find relevant materials

**Handling inconsistencies**: Different databases may use different computational settings, requiring care when combining data

**Attribution**: Properly cite databases and acknowledge their contributions to your work

**Contributing back**: When possible, contribute your own computed data to help the community

Databases are living resources that grow and improve over time, and community contributions make them more valuable for everyone.

---

**Previous Section**: [Section 7.3: AI/ML Toolkits for Materials Science](section_7.3.md)  
**Next Section**: [Section 7.5: Workflow Management Systems](section_7.5.md)

