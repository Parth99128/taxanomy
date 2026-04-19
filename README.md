# DeepDNA - AI-Driven eDNA Analysis Pipeline & Dashboard

Provides an innovative, unsupervised Deep Learning pipeline to identify taxonomic diversity directly from environmental DNA (eDNA), directly addressing the **MoES / CMLRE Problem Statement**.

## Overview 
This project abandons database-reliant pipelines (like QIIME2 or basic Random Forests) in favor of **Unsupervised Deep Learning** to analyze deep-sea eukaryotic taxa where reference databases are largely incomplete or misannotated. 

## Features
1. **Database-Independent Discovery:** Uses a PyTorch Variational Autoencoder (VAE) to learn sequence representations solely from raw sequence composition (latent space).
2. **Novel Taxa Clustering:** Applies HDBSCAN to the latent space to precisely group divergent sequences and flag them as potential "Unknown Deep-Sea Taxa", entirely circumventing the standard SILVA/PR2 databases.
3. **Optimized for Speed:** Vectorized PyTorch tensors run parallel computations on CPU/GPU, removing the massive alignment bottlenecks of BLAST or DADA2 mapping.
4. **Interactive Dashboard:** A Vite & React-powered frontend providing a high-fidelity interactive visualization of the pipeline results, latent space UMAPs, and diversity metrics across various ocean stations.

## File Structure
* `src/data_processing.py`: Prepares FASTA sequences and converts them into 4-channel one-hot numeric tensors for the Deep Learning models.
* `src/models.py`: Contains PyTorch architecture (`DNA_1D_CNN` and `DNA_VAE`) optimized for genomic sequence lengths.
* `src/pipeline.py`: Main executable that runs encoding, latent space compression, and density-based clustering to map taxonomy. It updates `pipeline_results.json`.
* `requirements.txt`: Required dependencies.
* `frontend/`: A full React application for interactive data visualization.

## Running the Pipeline (Backend)

1. Create a Python environment (`python -m venv venv`) and activate it.
2. Install dependencies:
```bash
pip install -r requirements.txt
```

Ensure you are in the root directory where your `deep_sea_18S_ASVs.fasta` file resides.

```bash
python -m src.pipeline
```

The script will:
* Train a VAE to learn the underlying genetics.
* Extract dense numeric vectors.
* Run Unsupervised clustering (HDBSCAN).
* Append to the `pipeline_results.json` log.
* Output novel species groupings to `novel_taxa_clusters.csv`.

## Running the Dashboard (Frontend)

To start the Vite React development server for the interactive dashboard:

1. Navigate to the `frontend` directory:
```bash
cd frontend
```
2. Install Node dependencies:
```bash
npm install
```
3. Start the dev server:
```bash
npm run dev
```

The interface will be available at `http://localhost:5173` (or the port specified in your terminal).