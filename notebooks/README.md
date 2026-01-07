# QSAR Workflow Notebooks

This directory contains a complete set of Jupyter notebooks that demonstrate a typical QSAR (Quantitative Structure-Activity Relationship) modeling workflow using ProQSAR.

## Overview

The notebooks are organized sequentially to guide you through the entire QSAR modeling process, from data compilation to applicability domain evaluation. Each notebook is self-contained but builds upon the outputs of previous notebooks.

## Workflow Steps

### 1. Data Compilation and Loading (`01_data_compilation.ipynb`)
- Load datasets with chemical structures (SMILES) and biological activities
- Perform data quality checks (missing values, duplicates, invalid SMILES)
- Conduct exploratory data analysis (EDA)
- Analyze molecular properties and activity distributions

**Outputs:** Validated dataset ready for featurization

### 2. Molecular Descriptor Calculation (`02_descriptor_calculation.ipynb`)
- Standardize SMILES strings using ProQSAR's standardization tools
- Calculate diverse molecular descriptors and fingerprints:
  - Morgan/ECFP fingerprints
  - RDKit fingerprints
  - MACCS keys
  - 2D molecular descriptors
- Analyze feature statistics and variance
- Save featurized dataset

**Outputs:** Feature matrix with calculated descriptors

### 3. Feature Selection (`03_feature_selection.ipynb`)
- Load featurized dataset
- Apply feature selection techniques:
  - Variance-based filtering
  - Correlation-based filtering
  - Model-based selection (Random Forest, Lasso)
- Identify most relevant descriptors
- Reduce dimensionality to avoid overfitting
- Analyze feature importance and correlations

**Outputs:** Reduced feature set optimized for modeling

### 4. Dataset Splitting (`04_dataset_splitting.ipynb`)
- Split dataset into training and test sets
- Use various splitting strategies:
  - Random splitting
  - Scaffold-based splitting (preserves chemical diversity)
  - Stratified splitting (preserves activity distribution)
- Visualize split quality using PCA
- Ensure proper representation of chemical space

**Outputs:** Training and test datasets

### 5. Model Building (`05_model_building.ipynb`)
- Build QSAR models using multiple algorithms:
  - Linear models (Ridge, Lasso, Elastic Net)
  - Partial Least Squares (PLS)
  - Tree-based models (Random Forest, XGBoost, CatBoost)
  - Support Vector Machines (SVM)
- Train models with cross-validation
- Compare model performance
- Analyze feature importance
- Save trained models

**Outputs:** Trained QSAR models

### 6. Model Validation (`06_model_validation.ipynb`)
- Internal validation using k-fold cross-validation
- External validation on independent test set
- Calculate performance metrics:
  - R² (coefficient of determination)
  - Q² (cross-validated R²)
  - RMSE (root mean squared error)
  - MAE (mean absolute error)
- Visualize prediction quality
- Analyze residuals and prediction errors

**Outputs:** Comprehensive validation results and predictions

### 7. Applicability Domain (`07_applicability_domain.ipynb`)
- Evaluate the applicability domain (AD) of the model
- Determine chemical space where predictions are reliable
- Use AD methods:
  - k-Nearest Neighbors (k-NN)
  - Local Outlier Factor (LOF)
  - One-Class SVM
- Identify compounds outside the AD
- Visualize AD boundaries using PCA
- Analyze prediction performance by AD status

**Outputs:** AD analysis and identification of reliable prediction space

## Getting Started

### Prerequisites

Make sure you have ProQSAR installed:

```bash
pip install proqsar
```

Or install from source:

```bash
git clone https://github.com/Medicine-Artificial-Intelligence/ProQSAR.git
cd ProQSAR
pip install -e .
```

### Running the Notebooks

1. Start Jupyter Notebook or JupyterLab:
   ```bash
   jupyter notebook
   # or
   jupyter lab
   ```

2. Navigate to the `notebooks/` directory

3. Open and run the notebooks in sequence, starting with `01_data_compilation.ipynb`

4. Each notebook will save outputs to the `Project/` directory, which will be created automatically

### Data Requirements

The example notebooks use the test dataset provided in `Data/testcase.csv`. To use your own data:

1. Prepare a CSV file with at least two columns:
   - SMILES strings (molecular structures)
   - Activity values (e.g., pIC50, pChEMBL)

2. Update the data loading paths in the notebooks to point to your dataset

3. Adjust column names if they differ from the defaults (`Smiles`, `pChEMBL`)

## Directory Structure

After running all notebooks, the following structure will be created:

```
ProQSAR/
├── notebooks/
│   ├── 01_data_compilation.ipynb
│   ├── 02_descriptor_calculation.ipynb
│   ├── 03_feature_selection.ipynb
│   ├── 04_dataset_splitting.ipynb
│   ├── 05_model_building.ipynb
│   ├── 06_model_validation.ipynb
│   ├── 07_applicability_domain.ipynb
│   └── README.md (this file)
├── Project/
│   ├── featurized_data.csv
│   ├── selected_features_data.csv
│   ├── train_data.csv
│   ├── test_data.csv
│   ├── cv_results.csv
│   ├── performance_comparison.csv
│   ├── test_predictions.csv
│   ├── test_ad_results.csv
│   ├── models/
│   │   └── best_model.pkl
│   └── ... (other outputs)
└── Data/
    └── testcase.csv
```

## Customization

### Featurizers

Modify the featurizer configuration in `02_descriptor_calculation.ipynb`:

```python
config = Config(
    featurizer={
        "feature_types": ["ECFP4", "RDK5", "MACCS", "RDKit2D"],
    }
)
```

### Feature Selection

Choose different feature selection methods in `03_feature_selection.ipynb`:

```python
config = Config(
    feature_selector={
        "method": "tree",  # Options: "tree", "lasso", "mutual_info", "none"
        "n_features_to_select": 50,
    }
)
```

### Splitting Strategies

Select different splitting methods in `04_dataset_splitting.ipynb`:

```python
config = Config(
    splitter={
        "split_type": "scaffold",  # Options: "random", "scaffold", "stratified"
        "test_size": 0.2,
    }
)
```

### Model Algorithms

Configure different models in `05_model_building.ipynb`:

```python
config = Config(
    model_developer={
        "models": ["rf", "xgb", "ridge", "lasso", "svr"],
        "task": "regression",
    }
)
```

### Applicability Domain

Choose AD methods in `07_applicability_domain.ipynb`:

```python
config = Config(
    applicability_domain={
        "method": "lof",  # Options: "knn", "lof", "ocsvm"
        "n_neighbors": 5,
    }
)
```

## Best Practices

1. **Data Quality**: Always start with high-quality, well-curated data
2. **Feature Selection**: Use feature selection to avoid overfitting and improve interpretability
3. **Cross-Validation**: Use cross-validation to assess model robustness
4. **External Validation**: Always validate on an independent test set
5. **Applicability Domain**: Check the AD before making predictions on new compounds
6. **Documentation**: Document your choices and parameters for reproducibility

## Troubleshooting

### Common Issues

1. **ImportError**: Make sure ProQSAR and all dependencies are installed
   ```bash
   pip install proqsar rdkit pandas scikit-learn matplotlib seaborn
   ```

2. **File Not Found**: Ensure you're running notebooks from the correct directory

3. **Memory Issues**: For large datasets, consider:
   - Using a subset of features
   - Processing data in batches
   - Using more memory-efficient featurizers

4. **RDKit Errors**: If you encounter RDKit-related errors, try:
   ```bash
   conda install -c conda-forge rdkit
   ```

## Additional Resources

- **ProQSAR Documentation**: [https://proqsar.readthedocs.io/en/latest/](https://proqsar.readthedocs.io/en/latest/)
- **ProQSAR GitHub**: [https://github.com/Medicine-Artificial-Intelligence/ProQSAR](https://github.com/Medicine-Artificial-Intelligence/ProQSAR)
- **QSAR Methodology**: See published literature on QSAR best practices

## Citation

If you use these notebooks or ProQSAR in your research, please cite:

```bibtex
@misc{proqsar2025,
  title = {ProQSAR: Automatic pipeline for QSAR modeling},
  author = {Tuyet-Minh Phan and Tieu-Long Phan and Phuoc-Chung Nguyen Van and contributors},
  year = {2025},
  howpublished = {\url{https://github.com/Medicine-Artificial-Intelligence/proqsar}}
}
```

## Contributing

Contributions to improve these notebooks are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your improvements
4. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.

## Support

For issues or questions:
- Open an issue on GitHub: [ProQSAR Issues](https://github.com/Medicine-Artificial-Intelligence/ProQSAR/issues)
- Refer to the documentation: [ProQSAR Docs](https://proqsar.readthedocs.io/en/latest/)
