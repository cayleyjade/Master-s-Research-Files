# Replications and Extensions of the MitoMammal: Investigating Skeletal Muscle Mitochondrial Metabolism in Type 2 Diabetes

 This file details the set up of the enviroment used to replicate and extend the applications of mitoMammal [`(Chapman et al., 2025)`](https://academic.oup.com/bioinformaticsadvances/article/5/1/vbae172/7876298).This enviroment was configured using Windows **Anaconda PowerShell Prompt**. The replication of these simulations was carried out using **VS Code**. The original mitoMammal authors alternatively document the use of the **Jupyter Notebook** browser interface in their [`README`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/README.md?ref_type=heads).

## Model Details 
Merging the curated frameworks of MitoCore and MitoMouse, mitoMammal presents a
dual-species genome-scale metabolic model, containing both human and mouse
gene-product-reaction rules for the in silico modelling of mitochondrial
metabolism and bioenergetic processes, such as oxidative phosphorylation (OXPHOS),
adenosine triphosphate (ATP) synthesis, and uncoupling protein 1 (UCP1)-mediated
uncoupling of OXPHOS. The resulting model contains 780 genes, 560 metabolic
reactions, and 445 metabolites. The dual-species framework of mitoMammal was developed to leverage experimental
insights obtained from murine-specific data to make inferences about human
pathophysiologies, addressing the gap in human data availability. A notable feature
of mitoMammal is its capacity to simulate mitochondrial metabolism beyond the
cardiomyocyte-specific context of its predecessors, as the model can be
contextualised using tissue-specific omics data. To achieve this, the original
authors modified the E-Flux2 algorithm, a constraint-based method for integrating
omics data into metabolic models. The modified algorithm allows the user to select
either human or mouse for omics data entry, and constrains the flux bounds of
reactions according to the species-specific gene-product-reaction rules. 

**Citation**: Chapman SP, Brunet T, Mourier A, Habermann BH. (2025). MitoMAMMAL: a genome scale
model of mammalian mitochondria predicts cardiac and BAT metabolism.
Bioinformatics Advances, 5(1): vbae172.
https://doi.org/10.1093/bioadv/vbae172
Repository: https://gitlab.com/habermann_lab/mitomammal

## Source Repository 
The mitoMammal model, core notebooks and datasets can be obtained from the official repository maintained by the Habermann Lab:
https://gitlab.com/habermann_lab/mitomammal

It is advised to clone the repository, as it contains the SBML model file `6_universal_mito_model.xml`, the integration algorithm file `efflux_method.py` and the various datasets.

| File/Folder | Description | Notebook Used |
|---|---|---|
| [`6_universal_mito_model.xml`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/6_universal_mito_model.xml?ref_type=heads) | The core mitoMammal metabolic model in SBML Level 3 (version 1) format. Built from mitoCore by translating human gene-product-reaction rules into mouse orthologs. Contains 780 genes, 560 metabolic reactions and 445 metabolites. | Loaded at the start of every notebook; required by all three. |
| [`efflux_method.py`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/efflux_method.py?ref_type=heads) | An adapted version of the E-Flux2 algorithm [`(Kim et al., 2016)`](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0157101), modified to allow the user to specify which organism (human or mouse) the integrated -omics data originates from, constraining only the reactions specific to that species. Used to integrate proteomic or transcriptomic data and constrain the model prior to parsimonious flux balance analysis (FBA), performed using COBRApy with the default GLPK solver.
| Glucose oxidation test *(validatory test mentioned in publication)* | Initial validation of mitoMammal's ability to predict accurate ATP yields. All nutrient input reactions except glucose and oxygen were constrained to zero to simulate aerobic glycolytic conditions, and ATP hydrolysis was maximised as the objective function.  | [`transcriptomic_human_BAT.ipynb`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/transcriptomic_human_BAT.ipynb?ref_type=heads) |
|[ `data/Mouse_Proteomic/Heart_data.tsv`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/data/Mouse_Proteomic/Heart_data.tsv?ref_type=heads) | Mitochondrial proteomic data isolated from mouse cardiac tissue (Hansen et al., 2024). Normalised protein counts scaled between 0 and 1, used to constrain mitoMammal via the E-Flux2 method while optimising ATP hydrolysis as the objective function (330 of 560 reactions constrained). | [`proteomics_mouse_cardiac_vs_BAT.ipynb`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/proteomics_mouse_cardiac_vs_BAT.ipynb?ref_type=heads) |
| [`data/Mouse_Proteomic/BAT_data.tsv`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/data/Mouse_Proteomic/BAT_data.tsv?ref_type=heads) | Mitochondrial proteomic data isolated from mouse brown adipose tissue (BAT) [`(Hansen et al., 2024)`](https://www.life-science-alliance.org/content/7/2/e202302147). Normalised protein counts scaled between 0 and 1, used to constrain mitoMammal via the E-Flux2 method while optimising the UCP1 reaction as the objective function (329 of 560 reactions constrained). | [`proteomics_mouse_cardiac_vs_BAT.ipynb`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/proteomics_mouse_cardiac_vs_BAT.ipynb?ref_type=heads) |
| [`data fBAT_vs_iPSC.csv` ](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/data/fBAT_vs_iPSC.csv?ref_type=heads)| Normalised RNA-sequencing data from an in vitro differentiated hiPSC-derived human brown adipocyte [`(Rao et al., 2023; GEO dataset GSE185623)`](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE185623). Read counts scaled between 0 and 1, used to constrain mitoMammal via the E-Flux2 method while optimising the UCP1 reaction as the objective function (488 of 560 reactions constrained). | [`transcriptomic_human_BAT.ipynb`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/transcriptomic_human_BAT.ipynb?ref_type=heads) |
| [`Chapman_etal_Table_S1a_tab.txt`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/Chapman_etal_TableS1a_tab.txt?ref_type=heads) | Supplementary Table S1a from [`(Chapman et al., 2025)`](https://academic.oup.com/bioinformaticsadvances/article/5/1/vbae172/7876298). Lists reaction attributes of the mitoMammal model, including gene-product-reaction rules and the orthology mapping between the 389 mouse and human gene pairs used to construct the model. | Used to verify reaction names, rules, and gene orthology assignments referenced within the notebooks. This dataset is not called directly by any notebook. |





## Installation
Set up a Python conda enviroment with the necessary dependencies, using conda (or pip) installation. 

### Step 1— Create & Activate mitoMammal Enviroment
```powershell
conda create --name [mitomammal] python=[3.8.20]
conda activate [mitomammal]
```
**Note**: Ensure the `mitomammal` enviroment is activated (you will see `(mitomammal) C:` at the start of a new prompt), before running `pip install`. If the enviroment is not activated, cobra will be installed into your base enviroment, not into mitomammal. 

### Step 2— Install Core Packages 
```powershell
conda install [jupyter] [numpy] [pandas] [matplotlib]
```
### Step 3— Install GLPK Solver 
```powershell
conda install -c conda-forge glpk
```
**Note:** The solver must be installed separately and before COBRApy. Verify that the simulations will run using this solver by running `print(cobra.Configuration().solver` in your notebook.

### Step 4— Install COBRApy v 0.29.0
```powershell
pip install [cobra]==[0.29.0]
```
**Note**: pip is used to install `COBRApy 0.29.0`, the version that was available when mitoMammal was published.

### Confirm Package Versions
```powershell
python --version
python -c "import cobra; print('COBRApy:', cobra.__version__)"
python -c "import numpy; print('NumPy:', numpy.__version__)"
python -c "import pandas; print('Pandas:', pandas.__version__)"
python -c "import matplotlib; print('Matplotlib:', matplotlib.__version__)"
python -c "import libsbml; print('python-libsbml:', libsbml.LIBSBML_DOTTED_VERSION)"
conda list swiglpk
```
**Expected Output**:
Python 3.8.20;
COBRApy 0.29.0;
NumPy 1.24.3;
Pandas 2.0.3;
Matplotlib 3.7.2;
python-libsbml 5.21.1;
swiglpk    5.0.13; 


## Using the mitoMammal
The replication of these simulations was carried out using **VS Code**, and this
is the approach documented throughout this README. Open the project folder in VS
Code and open any of the .ipynb files directly. Ensure that the [`efflux_method.py`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/efflux_method.py?ref_type=heads) notebook is in the same data directory as the notebook being run. 


Select the mitomammal `(Python 3.8.20)`
kernel from the kernel picker in the top-right corner of the notebook editor
before running any cells.

If the kernel does not appear in the list, ensure the
**Python extension** and **Jupyter extension** are installed in VS Code then use
Python: Select Interpreter from the Command Palette `(Ctrl+Shift+P)` to confirm
the mitomammal environment is available, and try selecting the kernel again.

Alternatively, the original mitoMammal authors document the use of the **Jupyter
Notebook** browser interface in their [`README`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/README.md?ref_type=heads), which can be launched as follows:
```powershell
jupyter notebook
```

This opens a local server in the browser, from which the available notebooks can
be opened and run.

Once a notebook is open with the correct kernel selected, in either environment,
run each cell in order. 

 ## General Workflow for Each Notebook

1. Load notebook & import the necessary packages: 
   
   ```powershell
   import matplotlib.pyplot as plt
   import pandas as pd
   import numpy as np
   import math
   import cobra 
   import os
   from os.path import join
   from cobra.util import create_stoichiometric_matrix
   from cobra.util.solver import linear_reaction_coefficients
   from cobra.io import read_sbml_model, write_sbml_model
   from cobra.flux_analysis import pfba
   from time import sleep
   from cobra.flux_analysis import flux_variability_analysis
   from efflux_method import *
   ```
2. Copy model file path and load the model [`6_universal_mito_model.xml`](https://gitlab.com/habermann_lab/mitomammal/-/blob/main/6_universal_mito_model.xml?ref_type=heads):
   ```powershell
   model = cobra.io.read_sbml_model(join("6_universal_mito_model.xml")) 
   ```

3. For relevant notebook, copy file path of datasets into:
   ```powershell
   result_Heart = perform_FBA("data/Proteomic/Heart_data.tsv",0,1,False,50,70)
   result_BAT = perform_FBA("data/Proteomic/BAT_data.tsv",0,1,False,50,420)
   data_fbat = pd.read_csv((join(data_dir, "fBAT_vs_iPSC.csv")))
   ```

4.  Run the notebook: Visualise & Compare to author's results: [`S1c_Reported fluxes`](https://academic.oup.com/bioinformaticsadvances/article/5/1/vbae172/7876298#supplementary-data)





