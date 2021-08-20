 # <p align="center"> Bonjour from Team Metagenomics Group A! :handshake:
  
![Visitor](https://visitor-badge.laobi.icu/badge?page_id=prembioinfo.Metagenomics-A)

![alt text](https://github.com/prembioinfo/Metagenomics-A/blob/main/METAGENOMICS.gif)
Spare some time to read this and make sure to peep into our work design. Trust us, you won't regret it!🤔

 
# <p align="center"> Metagenomic data analysis to detect antimicrobial resistance gene
We replicated the galaxy tutorial for the detection of antibiotic resistance: https://training.galaxyproject.org/training-material/topics/metagenomics/tutorials/plasmid-metagenomics-nanopore/tutorial.html
  
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

https://zenodo.org/record/3247504/files/RB01.fasta<br />
https://zenodo.org/record/3247504/files/RB02.fasta<br />
https://zenodo.org/record/3247504/files/RB03.fasta<br />
https://zenodo.org/record/3247504/files/RB04.fasta<br />
https://zenodo.org/record/3247504/files/RB05.fasta<br />
https://zenodo.org/record/3247504/files/RB06.fasta<br />
https://zenodo.org/record/3247504/files/RB07.fasta<br />
https://zenodo.org/record/3247504/files/RB08.fasta

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

| Datasets    | Task Performed                                                                                                                                                                             | Galaxy History Link of the results                                                                                                           | Name of the Person      |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------|
| RB01 & RB02 | Creating a history and building a list collection for the datasets by importing those datasets in history                                                                                  | https://usegalaxy.eu/u/ishabarve/h/imported-imported-imported-imported-imported-imported-imported-imported-imported-metagenomics--group-isha | Isha Barve              |
|             | Quality Control: using NanoPlot tool to explore the datasets                                                                                                                               |                                                                                                                                              | Morali Shah             |
|             | De-novo Assembly: Pairwise alignment using Map with minimap2 tool                                                                                                                          |                                                                                                                                              | Akanksha Kulkarni       |
|             | Ultrafast de novo assembly: using Miniasm tool                                                                                                                                             |                                                                                                                                              | Isha Barve              |
|             | Remapping: First convert the Assembly Graph (collection) created previously by Miniasm using GFA to Fasta tool then perform pairwise sequence alignment again using Map with minimap2 tool |                                                                                                                                              | Isha Barve              |
|             | Ultrafast consensus module using Racon tool                                                                                                                                                |                                                                                                                                              | Kartik A. Sawant        |
|             | Visualize the assemblies using Bandage image tool                                                                                                                                          |                                                                                                                                              | Isha Barve              |
|             | Optimizing assemblies using Create assemblies with Unicycler tool and again visualize using Bandage image tool                                                                             |                                                                                                                                              | Devika Kaliana          |
|             | Prediction of plasmid sequences and classes using PlasFlow tool                                                                                                                            |                                                                                                                                              | Morali Shah             |
|             | Scanning of genome contigs for antimicrobial resistance genes using staramr tool.                                                                                                          |                                                                                                                                              | Devika Kaliana          |
|             | Extract more information using CARD database                                                                                                                                               |                                                                                                                                              | Kartik A. Sawant        |
|             |                                                                                                                                                                                            |                                                                                                                                              |                         |
| RB03 & RB04 | Creating a history and building a list collection for the datasets by importing those datasets in history                                                                                  | https://usegalaxy.eu/u/priyankathedecoder/h/imported-imported-imported-imported-hackbio-internship-2021                                      | Rishikesh Dash          |
|             | Quality Control: using NanoPlot tool to explore the datasets                                                                                                                               |                                                                                                                                              | Rishikesh Dash          |
|             | De-novo Assembly: Pairwise alignment using Map with minimap2 tool                                                                                                                          |                                                                                                                                              | Sneha Das               |
|             | Ultrafast de novo assembly: using Miniasm tool                                                                                                                                             |                                                                                                                                              | Sneha Das               |
|             | Remapping: First convert the Assembly Graph (collection) created previously by Miniasm using GFA to Fasta tool then perform pairwise sequence alignment again using Map with minimap2 tool |                                                                                                                                              | Aishatu Muhammad Malami |
|             | Ultrafast consensus module using Racon tool                                                                                                                                                |                                                                                                                                              | Aishatu Muhammad Malami |
|             | Visualize the assemblies using Bandage image tool                                                                                                                                          |                                                                                                                                              | Priyanka Mishra         |
|             | Optimizing assemblies using Create assemblies with Unicycler tool and again visualize using Bandage image tool                                                                             |                                                                                                                                              | Priyanka Mishra         |
|             | Prediction of plasmid sequences and classes using PlasFlow tool                                                                                                                            |                                                                                                                                              | Priyanka Singh          |
|             | Scanning of genome contigs for antimicrobial resistance genes using staramr tool.                                                                                                          |                                                                                                                                              | Priyanka Singh          |
|             | Extract more information using CARD database                                                                                                                                               |                                                                                                                                              | Priyanka Singh          |
|             |                                                                                                                                                                                            |                                                                                                                                              |                         |
| RB05 & RB06 | Creating a history and building a list collection for the datasets by importing those datasets in history                                                                                  | https://usegalaxy.eu/u/shabs--/h/-hackbio-internship-2021---metagenomics-a                                                                   | Premnath                |
|             | Quality Control: using NanoPlot tool to explore the datasets                                                                                                                               |                                                                                                                                              | Halak Shukla            |
|             | De-novo Assembly: Pairwise alignment using Map with minimap2 tool                                                                                                                          |                                                                                                                                              | Lakshini Kannan         |
|             | Ultrafast de novo assembly: using Miniasm tool                                                                                                                                             |                                                                                                                                              | Lakshini Kannan         |
|             | Remapping: First convert the Assembly Graph (collection) created previously by Miniasm using GFA to Fasta tool then perform pairwise sequence alignment again using Map with minimap2 tool |                                                                                                                                              | Prathyusha Cota         |
|             | Ultrafast consensus module using Racon tool                                                                                                                                                |                                                                                                                                              | Prathyusha Cota         |
|             | Visualize the assemblies using Bandage image tool                                                                                                                                          |                                                                                                                                              | Premnath                |
|             | Optimizing assemblies using Create assemblies with Unicycler tool and again visualize using Bandage image tool                                                                             |                                                                                                                                              | Premnath                |
|             | Prediction of plasmid sequences and classes using PlasFlow tool                                                                                                                            |                                                                                                                                              | Shabnam S               |
|             | Scanning of genome contigs for antimicrobial resistance genes using staramr tool.                                                                                                          |                                                                                                                                              | Shabnam S               |
|             | Extract more information using CARD database                                                                                                                                               |                                                                                                                                              | Shabnam S               |
|             |                                                                                                                                                                                            |                                                                                                                                              |                         |
| RB07 & RB08 | Creating a history and building a list collection for the datasets by importing those datasets in history                                                                                  | https://usegalaxy.eu/u/ojonugwa/h/imported-metagenomic-project                                                                               | Rafat Omar              |
|             | Quality Control: using NanoPlot tool to explore the datasets                                                                                                                               |                                                                                                                                              | Rafat Omar              |
|             | De-novo Assembly: Pairwise alignment using Map with minimap2 tool                                                                                                                          |                                                                                                                                              | Rafat Omar              |
|             | Ultrafast de novo assembly: using Miniasm tool                                                                                                                                             |                                                                                                                                              | Marwa Amer              |
|             | Remapping: First convert the Assembly Graph (collection) created previously by Miniasm using GFA to Fasta tool then perform pairwise sequence alignment again using Map with minimap2 tool |                                                                                                                                              | Marwa Amer              |
|             | Ultrafast consensus module using Racon tool                                                                                                                                                |                                                                                                                                              | Marwa Amer              |
|             | Visualize the assemblies using Bandage image tool                                                                                                                                          |                                                                                                                                              | Nadeen Esmail           |
|             | Optimizing assemblies using Create assemblies with Unicycler tool and again visualize using Bandage image tool                                                                             |                                                                                                                                              | Nadeen Esmail           |
|             | Prediction of plasmid sequences and classes using PlasFlow tool                                                                                                                            |                                                                                                                                              | Nadeen Esmail           |
|             | Scanning of genome contigs for antimicrobial resistance genes using staramr tool.                                                                                                          |                                                                                                                                              | Ojonugwa Abubakar       |
|             | Extract more information using CARD database                                                                                                                                               |                                                                                                                                              | Ojonugwa Abubakar       |
  
  
  
 

Thank you for your patience and time!
Have a great day!🤗

![alt text](https://github.com/prembioinfo/Metagenomics-A/blob/main/Flowchart.png)
