-> AI-Driven Biodiversity Assessment from eDNA Data
This repository contains an end-to-end bioinformatics pipeline that uses unsupervised deep learning to analyze environmental DNA (eDNA) from deep-sea samples. The primary goal is to assess biodiversity, discover novel ecological groups, and identify potential new taxa directly from raw sequencing reads, minimizing the reliance on traditional reference databases.
The Challenge: The Unseen Majority in the Deep Sea
The deep ocean is a vast reservoir of undiscovered biodiversity. While eDNA offers a powerful, non-invasive window into these ecosystems, its analysis is severely hampered by a critical bottleneck: the lack of comprehensive reference databases for deep-sea organisms.
<details>
<summary><b>Click to expand the full problem statement</b></summary>
Title: Identifying Taxonomy and Assessing Biodiversity from eDNA Datasets
Background
The deep ocean, encompassing vast and remote ecosystems like abyssal plains, hydrothermal vents, and seamounts, harbors a significant portion of global biodiversity, much of which remains undiscovered due to its inaccessibility. Understanding deep-sea biodiversity is critical for elucidating ecological interactions (e.g., food webs, nutrient cycling), informing conservation strategies for vulnerable marine habitats, and identifying novel eukaryotic species with potential biotechnological or ecological significance.
Environmental DNA (eDNA) has emerged as a powerful, non-invasive tool for studying these ecosystems... offering insights into species richness and community structure.
Description
The Centre for Marine Living Resources and Ecology (CMLRE) will undertake routine voyages to the deep sea and collect sediment and water samples for biodiversity assessment...
However, assigning raw eDNA sequencing reads to eukaryotic taxa or inferring their ecological roles presents significant challenges, primarily due to the poor representation of deep-sea organisms in reference databases like SILVA, PR2, or NCBI... This dependency limits the discovery of new species and hinders accurate biodiversity assessments...
Expected Solution
To address the challenges of poor database representation and computational time in deep-sea eDNA analysis, we propose an AI-driven pipeline that uses deep learning and unsupervised learning to identify eukaryotic taxa and assess biodiversity directly from raw eDNA reads. The solution should be able to classify the sequences, annotate and estimate abundance. This solution minimizes reliance on reference databases, reduces computational time through optimized workflows, and enables the discovery of novel taxa and ecological insights in deep-sea ecosystems.
</details>
-> Our Solution: An AI-Driven Pipeline
This pipeline processes raw .fastq files and uses a series of data-driven steps to move from raw sequences to ecological insights.
![alt text](./images/final_report.png)

The Workflow
OTU Clustering (VSEARCH): Raw sequences are clustered into Operational Taxonomic Units (OTUs) at 97% sequence identity. This step quantifies the "species richness" of the sample and calculates the abundance of each OTU.
Data Vectorization (K-mer Tokenization): The representative DNA sequence of each OTU is converted into a numerical vector. This is done by counting the frequency of all possible 6-mers (6-base-pair "DNA words"), transforming biological sequences into a mathematical format suitable for AI.
Representation Learning (Variational Autoencoder - VAE): A deep learning model (VAE) is trained to compress the high-dimensional k-mer vectors (4096 features) into a dense, low-dimensional "embedding" (32 features). This embedding acts as a meaningful numerical fingerprint for each OTU, capturing its core genetic characteristics.
Ecological Group Discovery (HDBSCAN): Using the learned embeddings, the powerful HDBSCAN clustering algorithm automatically identifies dense groups of OTUs. These "ecological groups" represent clusters of organisms with similar genetic fingerprints. A key feature is its ability to identify "noise" points—OTUs that don't belong to any main group and are treated as potential novel candidates.
Novelty Quantification & Analysis:
Group Analysis: Each discovered group is profiled for its coherence, abundance, and a novelty_score that measures how distinct the group's overall genetic pattern is from the rest of the dataset.
Candidate Prioritization: Each unclustered "noise" OTU is assigned an individual novelty score. Those with the highest scores are flagged as High-Priority Novel Taxon Candidates.
Targeted Validation & Reporting: To save computational time and cost, the pipeline performs a "Smart BLAST". Instead of BLASTing all OTUs, it validates only a small, high-value subset:
A representative from each of the top 3 most abundant groups to identify the ecosystem's dominant players.
The top 3 most novel candidates to maximize the chance of confirming a new discovery.
Finally, a comprehensive report with text and visualizations is generated.
-> Key Features
Database-Independent Discovery: Identifies ecological structures and novel candidates without relying on potentially incomplete reference databases.
Novelty Prioritization: Uses a data-driven novelty_score to create a short, actionable list of the most promising OTUs for further investigation.
Computational Efficiency: Avoids the time-consuming process of BLASTing thousands of sequences by using a targeted "Smart BLAST" validation strategy.
End-to-End Workflow: Provides a complete solution from a raw .fastq file to a final, interpretable biodiversity report.
-> Getting Started
Prerequisites
Python 3.8+
VSEARCH 
The Python libraries listed in requirements.txt.
Installation
Clone the repository:
code
Bash
git clone https://github.com/YourUsername/YourRepoName.git
cd YourRepoName
Install the required Python packages:
code
Bash
pip install -r requirements.txt
(You will need to create a requirements.txt file containing libraries like: tensorflow, hdbscan, umap-learn, pandas, numpy, matplotlib, scikit-learn, biopython)
Running the Pipeline
Place your eDNA data file (e.g., sample.fastq) in the root directory of the repository.
Open the Biodiversity_Assessment_V3.ipynb notebook in Jupyter Lab/Notebook or Google Colab.
Ensure the input_file variable in the notebook points to your .fastq file.
Run the cells sequentially from top to bottom.
🔮 Future Work
Web Frontend: Develop a user-friendly web interface using Flask or FastAPI where users can upload their FASTQ files and view the report interactively.
Code Modularization: Refactor the core functions from the notebook into separate .py scripts for better maintainability and easier testing.
Advanced Validation: Incorporate internal cluster validation metrics (e.g., Silhouette Score) to mathematically quantify the quality of the discovered ecological groups.
📄 License
This project is licensed under the MIT License. See the LICENSE file for details.
