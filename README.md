# Multi-Bacteria Genomic Workflow README 
# 🦠 Multi-Bacteria Genomic Analysis Workflow

## 📌 Overview
Automated bioinformatics pipeline for bacterial pathogens including:
- *Staphylococcus aureus*
- *Klebsiella pneumoniae*
- *Escherichia coli*
- *Enterococcus faecalis*
- *Neisseria gonorrhoeae*

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
