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
 BELOW IS AN AUTOMATED BASH SCRIPT FOR ALL ANALYSI!
#!/bin/bash
# ============================================================
# Bacterial Genomic Analysis Pipeline
# Author: Beckley Brown
# Description: Automates QC, alignment, variant calling,
# annotation, resistance profiling, and phylogenetic analysis
# ============================================================

# ==== CONFIGURATION ====
THREADS=4
REF_GENOME="reference.fasta"   # Path to reference genome FASTA
SPECIES_NAME="bacteria_species"
OUTDIR="analysis_results"

# ==== TOOLS REQUIRED ====
# fastqc, trimmomatic, bwa, samtools, bcftools,
# prokka, abricate, roary, iqtree

mkdir -p $OUTDIR

# ==== 1. Quality Control ====
echo "[Step 1] Running Quality Control..."
mkdir -p $OUTDIR/qc_reports
for fq in *_1.fastq *_1.fastq.gz; do
    [ -f "$fq" ] || continue
    sample=$(basename "$fq" | sed 's/_1.fastq.*//')
    fastqc "$fq" "${sample}_2.fastq"* -o $OUTDIR/qc_reports
    # Trimmomatic
    trimmomatic PE -threads $THREADS \
        "${sample}_1.fastq.gz" "${sample}_2.fastq.gz" \
        "${OUTDIR}/${sample}_1_paired.fastq.gz" "${OUTDIR}/${sample}_1_unpaired.fastq.gz" \
        "${OUTDIR}/${sample}_2_paired.fastq.gz" "${OUTDIR}/${sample}_2_unpaired.fastq.gz" \
        SLIDINGWINDOW:4:20 MINLEN:50
done

# ==== 2. Genome Alignment ====
echo "[Step 2] Aligning reads with BWA-MEM..."
bwa index $REF_GENOME
mkdir -p $OUTDIR/alignment
for fq1 in $OUTDIR/*_1_paired.fastq.gz; do
    sample=$(basename "$fq1" | sed 's/_1_paired.fastq.gz//')
    fq2="$OUTDIR/${sample}_2_paired.fastq.gz"
    bwa mem -t $THREADS $REF_GENOME $fq1 $fq2 | samtools sort -@ $THREADS -o $OUTDIR/alignment/${sample}.bam
    samtools index $OUTDIR/alignment/${sample}.bam
done

# ==== 3. Variant Calling ====
echo "[Step 3] Calling variants..."
mkdir -p $OUTDIR/variants
for bam in $OUTDIR/alignment/*.bam; do
    sample=$(basename "$bam" .bam)
    samtools mpileup -uf $REF_GENOME $bam | bcftools call -mv -Oz -o $OUTDIR/variants/${sample}.vcf.gz
    bcftools index $OUTDIR/variants/${sample}.vcf.gz
done

# ==== 4. Annotation ====
echo "[Step 4] Annotating genomes..."
mkdir -p $OUTDIR/annotation
for bam in $OUTDIR/alignment/*.bam; do
    sample=$(basename "$bam" .bam)
    prokka --outdir $OUTDIR/annotation/${sample} --prefix ${sample} $REF_GENOME
done

# ==== 5. Resistance Profiling ====
echo "[Step 5] Screening for resistance genes..."
mkdir -p $OUTDIR/resistance
for bam in $OUTDIR/alignment/*.bam; do
    sample=$(basename "$bam" .bam)
    abricate --db card $REF_GENOME > $OUTDIR/resistance/${sample}_CARD.tab
    abricate --db resfinder $REF_GENOME > $OUTDIR/resistance/${sample}_ResFinder.tab
    abricate --db vfdb $REF_GENOME > $OUTDIR/resistance/${sample}_VFDB.tab
done

# ==== 6. Phylogenetic Analysis ====
echo "[Step 6] Building phylogenetic tree..."
mkdir -p $OUTDIR/phylogeny
# Core genome alignment using Roary (requires GFF files from Prokka)
roary -p $THREADS -f $OUTDIR/phylogeny $OUTDIR/annotation/*/*.gff
# IQ-TREE phylogeny
ALIGNMENT_FILE=$(find $OUTDIR/phylogeny -name "*.aln" | head -n 1)
iqtree -s $ALIGNMENT_FILE -nt AUTO -bb 1000 -alrt 1000

echo "[Done] Analysis complete! Results are in $OUTDIR"
