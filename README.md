# Multi-Bacteria Genomic Workflow README 
# 🦠 Multi-Bacteria Genomic Analysis Workflow

## 📌 Overview
Automated bioinformatics pipeline for bacterial pathogens including:
- *Staphylococcus aureus*
- *Klebsiella pneumoniae*
- *Escherichia coli*
- *Enterococcus faecalis*
- *Neisseria gonorrhoeae*
- Works for every bacteria*

---

## 🧰 Tools & Technologies
- **Bioinformatics Tools**: BWA, Samtools, Prokka, IQ-TREE, Roary, ABRicate, VCFtools, IGV
- **Programming**: Bash, Python, R
- **Databases**: CARD, ResFinder, VFDB
- **Environment**: Linux (Ubuntu)

---

## 📂 Data
- **Input**: Paired-end FASTQ reads for multiple bacterial species
- **Reference Genomes**: RefSeq FASTA for each species
- **Outputs**: Annotated genomes, resistance reports, phylogenetic trees

---

## ⚙️ Workflow Diagram
![Workflow](multi_bac_workflow.png)

---

## ⚙️ Step-by-Step Workflow
1. **Quality Control** – FastQC & Trimmomatic  
2. **Genome Alignment** – BWA-MEM per species  
3. **Variant Calling** – Samtools + BCFtools  
4. **Annotation** – Prokka per species  
5. **Resistance Profiling** – ABRicate (CARD, ResFinder, VFDB)  
6. **Phylogenetic Analysis** – Roary + IQ-TREE  
7. **Visualization** – Heatmaps, phylogenetic trees  

---

## 📊 Results
- Resistance gene detection across species  
- Comparative phylogenetic trees  
- AI-enhanced visualizations

---

## 📫 Contact
**Beckley Brown**  
📧 brownbeckley94@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/beckley-brown)


## 🛠 Installation & Setup

Below are commands to install the tools used in this project on a Linux (Ubuntu/Debian) system.  
If you're using a different OS, refer to the official documentation for each tool.

### 1️⃣ Update & Upgrade System
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential wget unzip git
# BWA
sudo apt install -y bwa

# Samtools
sudo apt install -y samtools

# BCFtools
sudo apt install -y bcftools

# VCFtools
sudo apt install -y vcftools

# Prokka
sudo apt install -y prokka

# IQ-TREE
sudo apt install -y iqtree

# Roary (via conda)
conda create -n roary_env roary -c bioconda -c conda-forge
conda activate roary_env

# ABRicate
sudo apt install -y abricate

# IGV
wget https://data.broadinstitute.org/igv/projects/downloads/IGV_2.17.4.zip
unzip IGV_2.17.4.zip
# CARD database for ABRicate
abricate --setupdb

# ResFinder database
abricate-get_db.py --db resfinder
abricate --setupdb
# R packages
sudo apt install -y r-base
R -e "install.packages(c('ggplot2', 'tidyverse', 'ape', 'gplots'))"

# Python packages
pip install biopython matplotlib pandas seaborn
conda install -c bioconda -c conda-forge bwa samtools bcftools vcftools prokka iqtree roary abricate
Work on your ubuntu Linux terminal
