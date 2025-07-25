
# Code and Analysis for: "Leveraging mutual information in Variational Autoencoders for improved dimensionality reduction of single-cell RNA sequencing data: The scInfoMaxVAE approach"

## System Requirements

  * Python 3.7+
  * Jupyter Notebook or Jupyter Lab
  * A GPU is recommended for efficient model training.

All required Python packages are listed in `requirements.txt`. They can be installed using pip:

```bash
pip install -r requirements.txt
```

## Directory Structure

The repository is organized as follows:

```
.
├── code/
│   ├── model.ipynb           # Notebook for VAE model training
│   └── pseudotime.ipynb    # Notebook for trajectory and pseudotime analysis
├── data/
│   ├── bash/
│   │   └── download_data.sh  # Script to download raw data
│   └── preprocess/
│       └── preprocess.py     # Script for data preprocessing
├── results/                    # Default output directory for model results and figures
└── README.md
```

-----

## Reproduction Workflow

The analysis is divided into three main steps.

### Step 1: Data Acquisition and Preprocessing

This initial step prepares the `AnnData` object for model training.

1.  **Download Data:** Navigate to the `data/bash/` directory and execute the download script.

    ```bash
    cd data/bash
    sh download_data.sh
    ```

    This script will download the raw dataset into the appropriate location.

2.  **Preprocess Data:** Next, run the preprocessing script to filter, normalize, and prepare the data.

    ```bash
    cd ../preprocess
    python preprocess.py
    ```

    **Output:** This will generate a preprocessed file, `[dataset_name].h5ad`, located in the `data/preprocessed_data/` directory (this directory will be created if it doesn't exist).

### Step 2: Model Training and Latent Space Generation

This step uses the preprocessed data to train the VAE and learn a low-dimensional representation of the cells.

1.  **Launch Jupyter** and open the `code/model.ipynb` notebook.

2.  **Configure Paths:** Before execution, verify that the file paths within the notebook are correctly configured.

      * The input `DATASET` variable should point to the `.h5ad` file generated in Step 1.
        ```python
        # e.g., DATASET = '../data/preprocessed_data/camp_data.h5ad'
        ```
      * The `output_path` should point to your desired location in the `results/` directory.
        ```python
        # e.g., output_path = '../results/camp_model_output.h5ad'
        ```

3.  **Execute Notebook:** Run all cells in the notebook sequentially.

4.  **Expected Output:** Upon completion, a new `.h5ad` file will be saved to the specified `output_path`. This file contains the original data supplemented with the learned latent space, stored in `adata.obsm['X_infomax']`. A 2D visualization of this latent space will also be generated.

### Step 3: Trajectory Inference and Visualization

This final step performs trajectory analysis on the learned latent space.

1.  **Open Notebook:** Open the `code/pseudotime.ipynb` notebook.

2.  **Configure Path:** Set the `DATASET` variable to the path of the output file from Step 2.

    ```python
    # e.g., DATASET = '../results/camp_model_output.h5ad'
    ```

3.  **Execute Notebook:** Run all cells sequentially. The notebook will perform Diffusion Map construction and calculate Diffusion Pseudotime (DPT).

4.  **Expected Outputs:** This notebook will generate and display several key visualizations for interpreting the cell trajectory:

      * **Diffusion Map Plot:** Visualizes the principal components of the diffusion map, revealing the main axes of variation and differentiation branches.
      * **Pseudotime Plot:** A 2D embedding of the cells colored by their calculated pseudotime, illustrating the inferred developmental progression.
      * **Violin Plot:** Shows the distribution of pseudotime values across different annotated cell types, which helps to validate the biological consistency of the inferred trajectory.