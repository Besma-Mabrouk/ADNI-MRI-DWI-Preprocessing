# ADNI-MRI-DWI-Preprocessing

This repository contains scripts and documentation for preprocessing brain MRI and DWI images obtained from the Alzheimer's Disease Neuroimaging Initiative (ADNI) database. The preprocessing pipeline is designed to handle 3D T1-weighted (T1W) imaging and two-dimensional echo planar DWI data, converting them into a standardized format for further analysis.

## Dataset

The data used in this project were obtained from the [ADNI database](https://www.adni.loni.usc.edu). ADNI is a public-private partnership launched in 2003, led by Principal Investigator Michael W. Weiner, MD. The goal of ADNI is to test the utility of MRI, PET, other biomarkers, and clinical and neuropsychological assessments in measuring the progression of mild cognitive impairment (MCI) and early Alzheimer’s disease (AD).

## Data Description

A total of 237 MRI and DWI brain images were used in this study. The data include clinical and demographic information, which is summarized in Table 1 of our associated publication. Images were chosen from different manufacturers to ensure a comprehensive dataset for testing the preprocessing pipeline.

## Preprocessing Pipeline

### 1. Conversion to NIfTI Format

- DICOM images are converted into NIfTI files using Heudiconv.
- Data is organized into structured directories (ANAT, DWI) following the Brain Imaging Data Structure (BIDS) format.

### 2. Denoising and Correction

- Denoising is applied to the raw diffusion data.
- Eddy current corrections are applied using the FSL tool to the b0 images acquired in the anterior-to-posterior (AP) direction.
- Bias field correction and skull stripping are performed using Advanced Normalization Tools (ANTs).

### 3. Tissue Segmentation and FOD Estimation

- Multi-shell multi-tissue constrained spherical deconvolution is conducted to generate fiber orientation densities (FOD).
- FODs are normalized for inter-subject comparison, focusing on two-tissue (WM, CSF) normalization for two-shells (b=0 and b=1000) DW images.

### 4. GM/WM Boundary Creation

- Anatomical images are converted to MRTRIX3 format and segmented into five tissue types.
- A grey matter/white matter boundary is created for seed analysis.

### 5. Streamline Generation and Connectivity Matrix

- Probabilistic tractography is used to generate streamlines, which are refined to create a weighted symmetric connectivity matrix (84x84).
- The matrix elements represent the normalized strength of connectivity between brain regions.

## Dependencies

The preprocessing pipeline utilizes the following software packages:

- [Heudiconv](https://github.com/nipy/heudiconv)
- [MRTRIX3](http://www.mrtrix.org)
- [FreeSurfer](http://surfer.nmr.mgh.harvard.edu/)
- [Advanced Normalization Tools (ANTs)](http://stnava.github.io/ANTs/)
- [FMRIB Software Library (FSL)](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/)

## How to Use

1. Clone this repository to your local machine.
2. Ensure all dependencies are installed.
3. Run the preprocessing script with the path to your raw DICOM data as an argument.

```bash
git clone https://github.com/yourusername/ADNI-MRI-DWI-Preprocessing.git
cd ADNI-MRI-DWI-Preprocessing
python preprocess.py /path/to/dicom/data
