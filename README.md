# 🧬 Transcriptomic Analysis of Ammonia-Treated HepG2 Cells Using Paired-End RNA Sequencing

---

##  1. Project Overview

This project performs transcriptomic analysis of ammonia-treated HepG2 human liver cancer cells using high-throughput RNA sequencing data. The study focuses on evaluating sequencing quality, alignment efficiency, and genome-wide transcriptional activity.

---

## 📂 2. Dataset Information

* **Accession ID:** SRR37250414
* **Source:** NCBI Sequence Read Archive (SRA)
* **Sample Type:** HepG2 human hepatocellular carcinoma
* **Organism:** Homo sapiens
* **Sequencing:** Paired-end RNA-seq (150 bp)
* **Platform:** Illumina NovaSeq 6000

---

##  3. Objectives

* Perform RNA-seq quality assessment
* Remove adapters and low-quality reads
* Align reads to reference genome
* Generate gene counts
* Analyze transcriptomic distribution across chromosomes

---

## 🛠️ 4. Tools & Purpose

| Tool                | Purpose                                 |
| ------------------- | --------------------------------------- |
| FastQC              | Assess raw and cleaned read quality     |
| Trimmomatic         | Remove adapters and low-quality bases   |
| HISAT2              | Align reads to reference genome         |
| SAMtools            | Convert, sort and index alignment files |
| FeatureCounts       | Quantify gene expression                |
| bam.iobio           | Alignment QC visualization              |
| UCSC Genome Browser | Genome-level visualization              |

---

##  5. RNA-seq Analysis Pipeline (Flowchart)

```
RAW FASTQ FILES
        ↓
FastQC (Before Trimming)
        ↓
Trimmomatic (Adapter Removal)
        ↓
FastQC (After Trimming)
        ↓
HISAT2 Alignment
        ↓
SAM → BAM Conversion
        ↓
Sorting & Indexing (SAMtools)
        ↓
FeatureCounts (Gene Quantification)
        ↓
Visualization (bam.iobio + UCSC)
        ↓
Statistical & Distribution Analysis
```

---

## 💻 6. Key Commands Used

###  FastQC (Before Trimming)

```bash
fastqc sample_R2.fastq
```

###  Trimming (Trimmomatic)

```bash
trimmomatic PE input_R1.fastq input_R2.fastq \
output_R1_paired.fastq output_R1_unpaired.fastq \
output_R2_paired.fastq output_R2_unpaired.fastq \
ILLUMINACLIP:adapters.fa:2:30:10 SLIDINGWINDOW:4:20 MINLEN:50
```

###  FastQC (After Trimming)

```bash
fastqc output_R1_paired.fastq
fastqc output_R2_paired.fastq
```

###  Alignment (HISAT2)

```bash
hisat2 -x genome_index -1 output_R1_paired.fastq -2 output_R2_paired.fastq -S aligned.sam
```

###  SAM to BAM

```bash
samtools view -bS aligned.sam > aligned.bam
```

###  Sorting

```bash
samtools sort aligned.bam -o sorted.bam
```

###  Indexing

```bash
samtools index sorted.bam
```

###  FeatureCounts

```bash
featureCounts -a annotation.gtf -o counts.txt sorted.bam
```

---

## 📊 7. FastQC Analysis

###  Before Trimming (Raw Reads)

* High base quality ✔
* Adapter contamination detected ❌
* Base composition bias (expected in RNA-seq)
* High duplication due to transcript abundance

 Interpretation: Good quality but requires cleaning

---

###  After Trimming (Clean Reads)

* Adapter contamination removed ✔
* High base quality maintained ✔
* GC distribution normal ✔
* Base bias persists (biological)

 Interpretation: High-quality dataset ready for analysis

---

### 🔹 Comparison Summary

| Feature         | Before  | After        |
| --------------- | ------- | ------------ |
| Base Quality    | Good    | Excellent    |
| Adapter Content | Present | Removed      |
| GC Content      | Normal  | Normal       |
| Bias            | Present | Biological   |
| Overall         | Usable  | High-quality |

---

## 📈 8. Alignment Results (bam.iobio)

* **Mapped Reads:** 96.7%
* **Proper Pairs:** 97.1%
* **Both Mates Mapped:** 98.1%
* **Duplicates:** 0%
* **Mapping Quality:** High

 Interpretation:

* Highly reliable alignment
* Minimal technical noise

---

## 🧬 9. Genome Browser Observations

* Strong alignment in exonic regions
* Peaks correspond to active genes
* Uniform genome coverage with hotspots

 Confirms biological transcription patterns

## 📊 10. Results: FeatureCounts-Based Transcriptomic Analysis

###  10.1 Dataset Overview

This RNA-seq dataset contains:

* **Total Chromosomes/Contigs:** 456
* **Total Reads Mapped:** 40,139,714
* **Total Gene Features Identified:** 1,947,954
* **Average Chromosome Length:** 7,037,908.12 bp
* **Average Reads per Chromosome:** 88,025.69
* **Average Genes per Chromosome:** 4,271.83

 Interpretation:
The dataset represents large-scale transcriptomic activity across genomic regions with substantial sequencing depth.

---

###  10.2 Reads Distribution Analysis

* Chromosome with **highest reads:** chr1
* Chromosome with **lowest reads:** chr14_GL000225v1_random

 Interpretation:

* Higher reads indicate active transcriptional regions
* Uneven distribution reflects biological variability rather than technical bias

---

###  10.3 Chromosome Length vs Reads

This analysis evaluates the relationship between chromosome size and read count.

 Interpretation:

* Proportional relationship → uniform sequencing
* Non-proportional → biological influence on expression

Observed pattern suggests **biological factors dominate read distribution**.

---

###  10.4 Read Density Analysis

**Read Density = Reads / Chromosome Length**

 Interpretation:

* High density → gene-rich and transcriptionally active regions
* Low density → less active or non-coding regions

This normalization removes chromosome size bias.

---

###  10.5 Gene Distribution Analysis

Gene counts vary significantly across chromosomes.

 Interpretation:

* High gene count → biologically important chromosomes
* Reflects structural and functional genome organization

---

###  10.6 Heatmap-Based Global Analysis

A multi-parameter heatmap was constructed using:

* Chromosome length
* Read counts
* Gene counts
* Read density

 Interpretation:

* Clustering of high-read chromosomes observed
* Distinct variation across genomic regions
* Indicates global transcriptomic heterogeneity

---

###  10.7 Top 20 Chromosomes Analysis

Chromosomes ranked using:
**Score = Reads + Genes + Read Density**

Top chromosomes include:
chr1, chrM, chr2, chr12, chr17, chr6, chr19, chr11, chr3, chr10

 Interpretation:

* High transcriptional activity
* High gene density
* Potential regulatory hotspots

---

###  10.8 Overall Biological Interpretation

* Gene expression is **non-uniform across chromosomes**
* Certain chromosomes dominate transcriptional activity
* Gene-rich regions correlate with higher reads
* Read density highlights active genomic zones

 Indicates:

* Functional specialization
* Presence of transcriptional hotspots

---

###  10.9 Conclusion of FeatureCounts Analysis

* RNA-seq reads are unevenly distributed
* Specific chromosomes show dominant transcription
* Gene density influences expression levels

 The dataset reflects biologically meaningful transcription patterns.

## 📈 11. Visualization and Data Representation

###  11.1 Reads Distribution Across Chromosomes

A bar plot was generated to visualize total reads mapped to each chromosome.

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("counts.txt", sep="\t")
reads_per_chr = df.groupby("Chr")["Counts"].sum()

reads_per_chr.plot(kind="bar")
plt.xlabel("Chromosome")
plt.ylabel("Total Reads")
plt.title("Reads Distribution Across Chromosomes")
plt.show()
```

 Interpretation:

* Chromosomes such as **chr1** show highest read counts
* Confirms uneven transcriptomic activity

---

###  11.2 Chromosome Length vs Reads Plot

A scatter plot was used to assess the relationship between chromosome length and reads.

```python
plt.scatter(df["Length"], df["Counts"])
plt.xlabel("Chromosome Length")
plt.ylabel("Reads")
plt.title("Chromosome Length vs Reads")
plt.show()
```

 Interpretation:

* Lack of strict proportionality indicates biological regulation
* Confirms non-uniform expression patterns

---

###  11.3 Read Density Visualization

Read density was calculated and visualized.

```python
df["ReadDensity"] = df["Counts"] / df["Length"]

df.plot.scatter(x="Length", y="ReadDensity")
plt.xlabel("Chromosome Length")
plt.ylabel("Read Density")
plt.title("Read Density Distribution")
plt.show()
```

 Interpretation:

* Highlights transcriptionally active regions
* Removes bias due to chromosome size

---

###  11.4 Heatmap Representation

Correlation heatmap generated for global comparison.

```python
import seaborn as sns

sns.heatmap(df[["Length","Counts","ReadDensity"]].corr(), annot=True)
plt.title("Correlation Heatmap")
plt.show()
```

 Interpretation:

* Reveals relationships between genomic features
* Identifies clusters of high transcriptional activity

---

###  11.5 Fragment Length and Mapping Quality (Alignment QC)

Visualization from alignment QC tools showed:

* Fragment length distribution centered around expected insert size
* High mapping quality (~60) for majority of reads

 Interpretation:

* Confirms high sequencing accuracy
* Supports reliability of downstream analysis

---

###  11.6 Coverage Distribution

Genome-wide read coverage plots indicate:

* Uniform baseline coverage
* Peaks corresponding to expressed genes

 Interpretation:

* Confirms successful transcript capture
* Identifies highly expressed regions




##  12. Biological Interpretation

* Gene expression is non-uniform
* Some chromosomes dominate transcription
* Gene-rich regions → higher reads
* Active genomic zones identified

---

## ✅ 13. Conclusion

* High-quality RNA-seq dataset
* Successful preprocessing and alignment
* Uneven gene expression across chromosomes
* Identified transcriptionally active regions

 Provides insight into genome organization and function

---

## 🔮 14. Future Prospects

* Differential expression analysis
* Pathway enrichment (GO, KEGG)
* Variant detection
* Machine learning-based biomarker discovery
* Multi-omics integration

---

## 📚 15. References

1. NCBI SRA Database
2. FastQC Documentation
3. HISAT2 Manual
4. SAMtools Documentation
5. FeatureCounts (Subread)
6. UCSC Genome Browser

---

## 👩‍🔬 Author

Riya Kundu
