 # <p align="center"> Bonjour from Team Metagenomics Group A! :handshake:
  
![Visitor](https://visitor-badge.laobi.icu/badge?page_id=prembioinfo.Metagenomics-A)

![alt text](https://github.com/prembioinfo/Metagenomics-A/blob/main/METAGENOMICS.gif)
Spare some time to read this and make sure to peep into our work design. Trust us, you won't regret it!🤔

 
# <p align="center"> Metagenomic data analysis to detect antimicrobial resistance gene
We replicated the galaxy tutorial for the detection of antibiotic resistance: https://training.galaxyproject.org/training-material/topics/metagenomics/tutorials/plasmid-metagenomics-nanopore/tutorial.html

## Ppt For Introduction:
	
Click here to view the Powerpoint Presentation > [Presentation](https://github.com/prembioinfo/Metagenomics-A/blob/main/Metagenomics%20Group%20A.ppt)
Click on view raw > To Watch The Presentation 
	
+ **Objective of the work:**
  * Perform Quality control on your reads
  * Assemble the genome
  * Determine the structure of the genome(s)
  * Determine the presence of potential antibiotic resistance genes.
  
Prerequisites: Create your account in Galaxy Europe server. (https://usegalaxy.eu/login)
  
## Step 0 - Preparing data
  
We used 8 samples from an original study (Li, R., M. Xie, N. Dong, D. Lin, X. Yang et al., 2018). Efficient generation of complete sequences of MDR-encoding plasmids by rapid assembly of MinION barcoding sequencing data. In the experiment, 12 MDR plasmid-bearing bacterial strains were selected for plasmid extraction, including E. coli, S. typhimurium, V. parahaemolyticus, and K. pneumoniae. Click here for [Datasets](https://doi.org/10.5281/zenodo.3247504)
  
## Step 1 -  Creating a history and building a list collection for the given datasets by importing those datasets in history

* Open your workspace in Galaxy. Make sure that you have an empty analysis history, Give it a name / Create a new history.
* Click the new-history icon at the top of the history panel. If the new-history is missing: Click on the galaxy-gear icon (History options) on the top of the history panel. Select the option Create New from the menu.
* Download the data on your pc from 
https://zenodo.org/record/3247504#.YRrL_IgzbIV
And upload
Or,
Import via links
Copy the links location:

| Samples | Species                 | Plasmid profile (kb)  | Links                                              |
|---------|-------------------------|-----------------------|----------------------------------------------------|
| RB01    | Escherichia coli        | 150,100               | https://zenodo.org/record/3247504/files/RB01.fasta |
| RB02    | Escherichia coli        | 160, 135, 100, 60, 40 | https://zenodo.org/record/3247504/files/RB02.fasta |
| RB03    | Escherichia coli        | 330, 60               | https://zenodo.org/record/3247504/files/RB03.fasta |
| RB04    | Escherichia coli        | 110, 130, 230         | https://zenodo.org/record/3247504/files/RB04.fasta |
| RB05    | Escherichia coli        | 150                   | https://zenodo.org/record/3247504/files/RB05.fasta |
| RB06    | Escherichia coli        | 250                   | https://zenodo.org/record/3247504/files/RB06.fasta |
| RB07    | Vibrio parahaemolyticus | 120                   | https://zenodo.org/record/3247504/files/RB07.fasta |
| RB08    | Salmonella typhimurium  | 340                   | https://zenodo.org/record/3247504/files/RB08.fasta |

Open the Galaxy Upload Manager (galaxy-upload on the top-right of the tool panel) and Select Paste/Fetch Data and copy those above link to retreive input data.<br />
(Tips: Creating a dataset collection - Click on Operations on multiple datasets (check box icon) at the top of the history panel > Check all the datasets in your history you would like to include > Click For all selected and choose Build dataset list > Enter a name for your collection > Click Create List to build your collection > Click on the checkmark icon at the top of your history again.)

## Step 2 - Quality Control: using NanoPlot tool to explore the datasets
- Search for Nanoplot Tool in the search box present on the left side:<br />
	
```
Set Parameters: Select "multifile mode” to batch
“Type of the file(s) to work on” to fasta 
“files” to  The Plasmids dataset collection you had created.
Click Execute.
```
## Step 3 - De-novo Assembly: Pairwise alignment using Map with minimap2 tool
- Search for Map with minimap2 Tool in the search box present on the left side:
```
Set Parameters:
Select 
“Will you select a reference genome from your history or use a built-in index?” to Use a genome from history and build index
“Use the following data collection as the reference sequence” to Plasmids dataset collection you had created.
“Single or Paired-end reads” to Single
“Select fastq dataset” to The Plasmids dataset collection
“Select a profile of preset options” to Oxford Nanopore all-vs-all overlap mapping
In the section Set advanced output options: “Select an output format” to paf
Click Execute.
```
## Step 4 - Ultrafast de novo assembly: using Miniasm tool
- Search for Miniasm Tool in the search box present on the left side:
```
Set Parameters:
Select
“Sequence Reads” to The Plasmids dataset collection
“PAF file” to Output Minimap dataset collection created by Minimap2 tool
Click Execute.
```
## Step 5 - Remapping: First convert the Assembly Graph (collection) created previously by Miniasm using GFA to Fasta tool then perform pairwise sequence alignment again using Map with minimap2 tool

- Search for GFA to Fasta Tool in the search box present on the left side:
```
Set Parameters:
Select
“Input GFA file” to the Assembly Graph (collection) created by Miniasm
Click Execute. 
```
- Search for Map with minimap2 Tool in the search box present on the left side:
```
Set Parameters:
Select
“Will you select a reference genome from your history or use a built-in index?” to Use a genome from history and build index
“Use the following dataset as the reference sequence” to FASTA file output from GFA to Fasta tool (collection)
“Single or Paired-end reads” to single
“Select fastq dataset” to The Plasmids collection
“Select a profile of preset options” to PacBio/Oxford Nanopore read to reference mapping (-Hk19)
In the section Set advanced output options:
“Select an output format” to paf
Click Execute.

```

## Step 6: Ultrafast consensus module using Racon tool
- Search for Racon Tool in the search box present on the left side:
```
Set Parameters:
 Select
“Sequences” to The Plasmids dataset collection
“Overlaps” to the latest PAF file collection created by Minimap2 tool
“Target sequences” to the FASTA file collection created by GFA to Fasta tool.
Click Execute.

```
## Step 7: Visualize the assemblies using Bandage image tool
- Search for Bandage image Tool in the search box present on the left side:
```
Set Parameters:
Select
“Graphical Fragment Assembly” to the Assembly graph collection created by Miniasm tool
Click Execute.

```
- Explore galaxy-eye for  the output images.

## Step 8: Optimizing assemblies using Create assemblies with Unicycler tool and again visualize using Bandage image tool
- Search for Create assemblies with Unicycler Tool in the search box present on the left side:
```
Set Parameters:
Select
“Paired or Single end data” to None
“Select long reads. If there are no long reads, leave this empty” to The Plasmids dataset collection
Click Execute.

```
- Search for Bandage image Tool in the search box present on the left side:
```
Set Parameters:
Select
“Graphical Fragment Assembly” to the Final Assembly Graph collection created by Unicycler tool
Click Execute. 

```
- Explore galaxy-eye for  the output images again.
- Use the Scratchbook (galaxy-scratchbook) to compare the  assemblies for the Bandage tool images for the assemblies: The assembly that you got from running minimap2, miniasm, racon tool (first time you ran bandage) with the assembly obtained with Unicycler tool.
## Step 9: Prediction of plasmid sequences and classes using PlasFlow tool
- Search for PlasFlow Tool in the search box present on the left side:
```
Set Parameters:
Select
“Sequence Reads” to the Final Assembly collection created by Unicycler
Click Execute.

```

## Step 10: Scanning of genome contigs for antimicrobial resistance genes using staramr tool
- Search for staramr Tool in the search box present on the left side:
```
Set Parameters:
Select
“genomes” to the Final Assembly collection created by Unicycler
Click Execute.

```
## Step 11: Extract more information using CARD database (Comprehensive Antibiotic Resistance Database)
- Use the summary file to search the database to get more information about the obtained antibiotic resistant genes. Hence head towards the CARD database (Comprehensive Antibiotic Resistance Database). 

**Analyze the results.**
	
## Contributors:

We created 4 subgroups to distribute the analysis of the total 8 datasets. The datasets contained raw long sequencing data (reads), sequenced by the Nanopore MinION sequencer, extracted from different MDR plasmid-bearing bacterial strains. (Li et al. 2018) Each subgroup worked with 2 datasets and analysed it following the steps mentioned in the referenced galaxy tutorial. The steps for the analysis was distributed among each member according to the subgroup. Click here to view the detailed contribution report > [Contributors](https://github.com/prembioinfo/Metagenomics-A/blob/main/Contibution%20details.csv)

  
| Name                    | Slack ID       | Email Id (Galaxy)                          |
|-------------------------|----------------|--------------------------------------------|
| Priyanka Singh          | @Decoder       | priyankathedecoder@gmail.com               |
| Premnath                | @Premnath      | batchaamadan17@gmail.com                   |
| Rishikesh Dash          | @Rishikesh     | rishikeshdash50@gmail.com                  |
| Sneha Das               | @Snehabioinfo  | snneha26@gmail.com                         |
| Priyanka Mishra         | @Priyanka      | priyankam9998@gmail.com                    |
| Isha Barve              | @WandaMaximoff | isha.barve0@gmail.com                      |
| Morali Shah             | @morali        | shahmorali@gmail.com                       |
| Akanksha Kulkarni       | @MoonlitRose99 | gmsbin15.akanksha.kulkarni@gnkhalsa.edu.in |
| Kartik A. Sawant        | @Venomm        | kartikatulsawant14@gmail.com               |
| Devika Kaliana          | @BARBARELLA    | devikakaliana94@gmail.com                  |
| Aishatu Muhammad Malami | @AishatuMM     | ummiamal@gmail.com                         |
| Halak Shukla            | @Halak         | halakshukla0101@gmail.com                  |
| Lakshini Kannan         | @Lakshini      | lakshinikannan@gmail.com                   |
| Prathyusha Cota         | @Prathyusha    | Prathyushacota@gmail.com                   |
| Shabnam S               | @Shabnam       | shab31201@gmail.com                        |
| Rafat Omar              | @RafatOmar     | rafat.0mar@outlook.com                     |
| Marwa Amer              | @Marwa         | marwa.amer@must.edu.eg                     |
| Nadeen Esmail           | @Nadeen        | nadeenesmail4@gmail.com                    |
| Ojonugwa Abubakar       | @ojay          | otabu01@gmail.com                          |
 
Thank you for your patience and time!
Have a great day!🤗

![alt text](https://github.com/prembioinfo/Metagenomics-A/blob/main/Flowchart.png)
	
### References
1. Antibiotic resistance detection. Galaxy Training by AvatarWillem de Koning AvatarSaskia Hiltemann.
2. Li, R., Xie, M., Dong, N., Lin, D., Yang, X., Wong, M. H. Y., ... & Chen, S. (2018). Efficient generation of complete sequences of MDR-encoding plasmids by rapid assembly of MinION barcoding sequencing data. Gigascience, 7(3), gix132.
3. Wick, R. R., M. B. Schultz, J. Zobel, and K. E. Holt, 2015 Bandage: interactive visualization of de novo genome assemblies. Bioinformatics 31: 3350–3352. 10.1093/bioinformatics/btv383
4. Li, H., 2016 Minimap and miniasm: fast mapping and de novo assembly for noisy long sequences. Bioinformatics 32: 2103–2110. 10.1093/bioinformatics/btw152
5. Jia, B., A. R. Raphenya, B. Alcock, N. Waglechner, P. Guo et al., 2016 CARD 2017: expansion and model-centric curation of the comprehensive antibiotic resistance database. Nucleic Acids Research 45: D566–D573. 10.1093/nar/gkw1004
6. Vaser, R., I. Sović, N. Nagarajan, and M. Šikić, 2017 Fast and accurate de novo genome assembly from long uncorrected reads. Genome Research 27: 737–746. 10.1101/gr.214270.116
7. Wick, R. R., L. M. Judd, C. L. Gorrie, and K. E. Holt, 2017 Unicycler: Resolving bacterial genome assemblies from short and long sequencing reads (A. M. Phillippy, Ed.). PLOS Computational Biology 13: e1005595. 10.1371/journal.pcbi.1005595.
8. Krawczyk, P. S., L. Lipinski, and A. Dziembowski, 2018 PlasFlow: predicting plasmid sequences in metagenomic data using genome signatures. Nucleic Acids Research 46: e35–e35. 10.1093/nar/gkx1321.
9. Coster, W. D., S. D’Hert, D. T. Schultz, M. Cruts, and C. V. Broeckhoven, 2018 NanoPack: visualizing and processing long-read sequencing data (B. Berger, Ed.). Bioinformatics 34: 2666–2669. 10.1093/bioinformatics/bty149.
10. Li, H., 2018 Minimap2: pairwise alignment for nucleotide sequences (I. Birol, Ed.). Bioinformatics 34: 3094–3100. 10.1093/bioinformatics/bty191.
11. Maio, N. D., L. P. Shaw, A. Hubbard, S. George, N. Sanderson et al., 2019 Comparison of long-read sequencing technologies in the hybrid assembly of complex bacterial genomes. 10.1101/530824.
12. Zankari E, Hasman H, Cosentino S, Vestergaard M, Rasmussen S, Lund O, Aarestrup FM, Larsen MV. 2012. Identification of acquired antimicrobial resistance genes. J. Antimicrob. Chemother. 67:2640–2644. doi: 10.1093/jac/dks261
13. Zankari E, Allesøe R, Joensen KG, Cavaco LM, Lund O, Aarestrup F. PointFinder: a novel web tool for WGS-based detection of antimicrobial resistance associated with chromosomal point mutations in bacterial pathogens. J Antimicrob Chemother. 2017; 72(10): 2764–8. doi: 10.1093/jac/dkx217
14. Carattoli A, Zankari E, Garcia-Fernandez A, Voldby Larsen M, Lund O, Villa L, Aarestrup FM, Hasman H. PlasmidFinder and pMLST: in silico detection and typing of plasmids. Antimicrob. Agents Chemother. 2014. April 28th. doi: 10.1128/AAC.02412-14

