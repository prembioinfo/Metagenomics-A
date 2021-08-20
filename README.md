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


  
  
  
 

Thank you for your patience and time!
Have a great day!🤗

![alt text](https://github.com/prembioinfo/Metagenomics-A/blob/main/Flowchart.png)
