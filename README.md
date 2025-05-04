# Fit_models_and_predict_PINN_to_experimental_data_soft_tussie
## Hyperelastic Tissue Modeling Framework

## Installation

To run this framework, you'll need Python 3.7 or later installed on your machine. You can download Python from the official website: https://www.python.org/

Install the required Python libraries using pip:
```bash
pip install numpy pandas matplotlib scipy tensorflow scikit-learn
```

## Usage

1. Clone the repository to your local machine:
```bash
git clone https://github.com/karinaurazova/Fit_models_and_predict_PINN_to_experimental_data_soft_tussie
```

2. The main components are:
- `Fitting_models_to_experimental_data_soft_tissue_and_PINN_predict.ipynb`: Main script containing the complete framework
- `sample_data/`: Directory with example tissue datasets
- `results/`: Output directory for generated plots and data

3. To run the complete analysis pipeline:
```bash
python Fitting_models_to_experimental_data_soft_tissue_and_PINN_predict.ipynb
```

## Theory

### Hyperelasticity Fundamentals
The framework models tissue mechanics using strain energy density functions:

```math
Ψ = Ψ_{\text{iso}} + Ψ_{\text{aniso}}
```

**Common Models**:
1. Neo-Hookean:
```math
Ψ = C_1(I_1 - 3)
```

2. Mooney-Rivlin:
```math
Ψ = C_1(I_1 - 3) + C_2(I_2 - 3)
```

3. Ogden Anisotropic:
```math
Ψ = ∑_{i=1}^N \frac{μ_i}{α_i}(λ_1^{α_i} + λ_2^{α_i} + λ_3^{α_i} - 3) + Ψ_{\text{fiber}}
```

### Tissue-Specific Modeling
The framework automatically selects models based on tissue properties:

| Tissue Type          | Preferred Models                          | Fiber Handling               |
|----------------------|------------------------------------------|------------------------------|
| GI Tract Organs      | Mooney-Rivlin 3-param, Yeoh 3rd order    | Circular + longitudinal      |
| Skin                 | Neo-Hookean, Ogden Isotropic             | Random orientation           |
| Anisotropic Tissues  | HGO, Anisotropic Ogden                   | Direction-specific           |

### Neural Network Approach
The Physics-Informed Neural Network architecture:

```
Input Layer (Strain)
│
├─ Dense Layer (128 neurons, tanh)
│
├─ Dense Layer (128 neurons, tanh)
│
├─ Dense Layer (128 neurons, tanh)
│
└─ Output Layer (Stress)
```

## Key Features

1. **Automated Workflow**:
```python
def process_material(file_path):
    # 1. Load and preprocess data
    # 2. Generate synthetic replicates
    # 3. Calculate average curve
    # 4. Fit hyperelastic models
    # 5. Train neural network
    # 6. Save results
```

2. **Specialized Material Models**:
```python
def hgo_model(strain, k1, k2, kappa=0.05):
    lmbda = 1 + strain/100
    I1 = lmbda**2 + 2/lmbda
    I4 = kappa*I1 + (1-3*kappa)*lmbda**2
    return 2*(lmbda - 1/lmbda**2)*(1 + 2*k1*(I4-1)*np.exp(k2*(I4-1)**2)*(1-3*kappa))
```

3. **Comparative Analysis**:
```python
def fit_and_plot_models(strain, stress, tissue_props):
    # Tests 6+ models and visualizes comparisons
    # Returns error metrics for each model
```

## Example Outputs

### Model Fitting Comparison
![Model Comparison Plot](https://github.com/karinaurazova/Fit_models_and_predict_PINN_to_experimental_data_soft_tussie/blob/main/results/result.png)

### Neural Network Prediction
![PINN Prediction](https://github.com/karinaurazova/Fit_models_and_predict_PINN_to_experimental_data_soft_tussie/blob/main/results/result1.png)

## Supported Tissues

Pre-configured tissue properties include:

| Tissue               | Structure                              | Anisotropy | Fiber Direction       |
|----------------------|----------------------------------------|------------|-----------------------|
| Mouse Esophagus      | Stratified squamous epithelium        | Moderate   | Circular              |
| Mouse Skin           | Stratified epithelium + connective    | Weak       | Random                |
| Mouse Large Intestine| Simple columnar epithelium            | High       | Longitudinal + circular |

To add custom tissues:

```python
TISSUE_PROPERTIES['new_tissue.csv'] = {
    'name': 'Custom Tissue',
    'type': 'organ_type',
    'structure': 'tissue_structure_description',
    'anisotropy': 'weak/moderate/high',
    'fiber_direction': 'preferred_orientation'
}
```

## Data Format

Input CSV files should contain:
```csv
Strain;Stress
0.0;0.0
1.2;0.15
2.8;0.32
...
```

## Command Line Options

```bash
usage: Fitting_models_to_experimental_data_soft_tissue_and_PINN_predict.ipynb [-h] [--samples N] [--noise LEVEL] [--epochs N]

optional arguments:
  -h, --help     show this help message and exit
  --samples N    Number of synthetic samples (default: 20)
  --noise LEVEL  Noise level for synthetic data (default: 0.03)
  --epochs N     Training epochs for neural network (default: 1000)
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

For bug reports or feature requests, please open an issue.

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/karinaurazova/Fit_models_and_predict_PINN_to_experimental_data_soft_tussie/blob/main/LICENSE) file for details.

## Citation

If you use this framework in your research, please cite:

```bibtex
@software{HyperelasticTissueModeling,
  author = {Karina Urazova},
  title = {Hyperelastic Tissue Modeling Framework},
  year = {2025},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/karinaurazova/Fit_models_and_predict_PINN_to_experimental_data_soft_tussie}}
}
```

## Contact

For questions or collaborations:
- Email: karina_urazova@icloud.com

***

**This framework continues to evolve - star the repository to stay updated on new features and improvements!**

