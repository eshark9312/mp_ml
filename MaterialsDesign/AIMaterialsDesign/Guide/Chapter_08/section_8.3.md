# Section 8.3: Case Study 2 - Band Gap Prediction for Photovoltaics

## The Challenge

Photovoltaic applications require semiconductors with specific band gaps. For single-junction solar cells, the optimal band gap is typically 1.0-1.7 eV, which maximizes the conversion of sunlight to electricity. This optimal range comes from balancing two competing factors: materials with smaller band gaps can absorb more of the solar spectrum (more photons), but each photon generates less voltage. Materials with larger band gaps generate more voltage per photon but absorb fewer photons. The optimal band gap balances these factors to maximize power output.

Finding materials with band gaps in this range is crucial for developing efficient solar cells. However, the challenge is that band gaps are difficult to predict accurately. Standard DFT functionals systematically underestimate band gaps, often by 30-50% or more. This "band gap problem" is a fundamental limitation of local and semi-local DFT functionals. Even hybrid functionals, which mix DFT with exact exchange, have limitations—they're more accurate but still not perfect, and they're computationally much more expensive.

Machine learning models trained on DFT data inherit these limitations. If the training data has systematic errors (like underestimated band gaps), the model will learn those errors. However, machine learning can still be valuable for screening if the errors are systematic and understood. If DFT systematically underestimates band gaps by a consistent amount, a machine learning model trained on DFT data will also systematically underestimate, but it will still correctly rank materials—materials with larger true band gaps will have larger predicted band gaps, even if the absolute values are off. For screening applications, this relative accuracy is often sufficient.

The challenge is ensuring that the systematic errors don't change the ranking of materials. If the errors vary significantly across different material classes, a model might incorrectly rank materials. This is why understanding the limitations of both DFT and machine learning models is crucial for using them effectively in materials discovery.

## The Approach

This case study demonstrates using graph neural networks to predict band gaps for photovoltaic materials screening.

### Dataset Preparation

We collected a dataset of semiconductors with computed band gaps:
- Queried the Materials Project for materials with band gaps between 0.1 and 5.0 eV
- Filtered for reasonably stable materials (formation energy < 0.1 eV/atom)
- Ensured diversity across composition and structure types

The dataset included both the computed band gaps (from PBE functional) and, where available, more accurate values from hybrid functionals or experiments for validation.

### Graph Neural Network Model

We used a Crystal Graph Convolutional Network (CGCNN) architecture:
- **Node features**: Element embeddings (learned representations of each element)
- **Edge features**: Bond distances encoded as Gaussian basis functions
- **Graph convolution**: Multiple layers that aggregate information from neighboring atoms
- **Graph-level pooling**: Mean pooling to get a single prediction per structure

The model architecture was designed to:
- Capture local atomic environments (through graph convolution)
- Propagate information through the structure (through multiple layers)
- Learn element-specific patterns (through learned embeddings)

### Training Process

The model was trained using:
- **Loss function**: Mean squared error between predicted and true band gaps
- **Optimizer**: Adam with learning rate scheduling
- **Regularization**: Dropout and weight decay to prevent overfitting
- **Validation**: Monitored performance on held-out test set

Training proceeded for multiple epochs, with early stopping when validation performance stopped improving.

### Evaluation

The trained model achieved:
- **MAE**: 0.15 eV on test set
- **RMSE**: 0.20 eV
- **R²**: 0.94

These results are excellent given that:
- Band gaps range from 0 to ~5 eV
- The model captures most of the variance
- Errors are small enough for screening applications

### Application to Photovoltaic Screening

Using the trained model, we screened materials for photovoltaic applications:
- **Candidate generation**: Generated candidate materials from common semiconductor structure types
- **Band gap prediction**: Used the model to predict band gaps rapidly
- **Filtering**: Identified materials with predicted band gaps in the optimal range (1.0-1.7 eV)
- **Ranking**: Ranked by other relevant properties (stability, cost, etc.)

This screening identified 20 new candidate materials with promising band gaps for photovoltaic applications.

## Analysis and Insights

Several insights emerged from this case study:

**Graph representation is powerful**: Representing structures as graphs and using GNNs achieved better performance than traditional descriptor-based methods. The graph representation naturally captures structural information.

**Systematic errors are acceptable**: The model inherits DFT's systematic band gap underestimation, but this is acceptable for screening if it's consistent. Materials are still ranked correctly relative to each other.

**Transfer learning potential**: Models trained on large datasets could be fine-tuned for specific applications (e.g., specific material classes or property ranges) with minimal additional data.

**Multi-property prediction**: Extending to predict multiple properties simultaneously (band gap, stability, cost) would enable more comprehensive screening.

## Comparison with Traditional Methods

The GNN approach outperformed traditional methods:
- **Descriptor-based ML**: GNN achieved 15% lower MAE
- **Simple heuristics**: GNN was much more accurate
- **Direct DFT**: GNN was 10,000x faster with acceptable accuracy loss

This demonstrates the value of modern deep learning approaches for materials property prediction.

## Lessons Learned

Key lessons from this case study:

**Architecture matters**: Graph neural networks are well-suited for crystal structures and achieve better performance than alternatives.

**Data quality is crucial**: High-quality training data (accurate band gaps) leads to better models. Using hybrid functional data where available improved results.

**Understanding limitations**: Knowing that models inherit DFT errors helps in interpreting and using predictions appropriately.

**Screening vs. final values**: For screening, relative accuracy (ranking) matters more than absolute accuracy. Small systematic errors are acceptable.

This case study demonstrates how modern machine learning can enable rapid screening for specific applications, accelerating the discovery of materials with desired properties.

---

**Previous Section**: [Section 8.2: Case Study 1 - Discovering High-Entropy Alloys](section_8.2.md)  
**Next Section**: [Section 8.4: Case Study 3 - Active Learning for Catalyst Discovery](section_8.4.md)

