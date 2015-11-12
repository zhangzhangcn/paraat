

# Documentation #

ParaAT (Parallel Alignment and back-Translation) is capable of manipulating a large number of homologous groups. ParaAT features parallel construction of multiple protein-coding DNA alignments and thus is well suited for large-scale data analysis in the high-throughput era.

  * Back-translated nucleotide alignments guided by amino acid alignments are more reliable and accurate than direct nucleotide alignments.
  * Extant tools developed for automating back-translated nucleotide alignments accept only one homologous group as input and thus are incapable of processing a large number of homologs.
  * Constructing multiple homologous alignments for protein-coding DNA sequences is crucial for a variety of bioinformatic analyses but remains computationally challenging.
  * With the growing amount of sequence data and the ongoing efforts largely dependent on protein-coding DNA alignments, there is an increasing demand for a tool capable of processing a large number of homologous groups.

ParaAT involves two parallel regions, in which the master thread creates a team of slave threads running concurrently. In the first parallel region, ParaAT aligns protein sequences for multiple homologs by assigning each homolog to one of the slave threads. In the second region, ParaAT parallelly back-translates protein sequence alignments into the corresponding DNA alignments.

ParaAT is well suited for large-scale data analysis in the high-throughput era, providing good scalability and exhibiting high parallel efficiency for computationally demanding tasks.

# Usage Notes #

  * The input data for ParaAT are:
    1. relationships of multiple homologous groups;
    1. the number of processors;
    1. fasta-formatted nucleotide and amino acid sequences.

  * Homologous groups
> > To ease the input of multiple homologous groups, ParaAT accepts a tab-delimited text file with each row representing a homologous group. An example data file containing 9,712 human-chimp-mouse homologs is available at [here](http://code.google.com/p/paraat/downloads/list). For instance,
```
NP_000005       NP_783327       XP_001139819
NP_000006       NP_032699       XP_001146758
NP_000008       NP_031409       XP_001162935
.......
```
> > There is no restriction on the number of homologous groups (dependent on the memory size of computer used). In addition, different rows (groups) can have different numbers of homologous genes.

  * ParaAT offers user-customized options to output the resulting alignments into several different formats (axt | fasta | paml | codon | clustal), aiming to facilitate downstream analyses—typically quantification of selective pressure acting on genes and estimation of synonymous and nonsynonymous substitution rates.

  * Sequence IDs in amino acid file, nucleotide file, and homologous group file should be the same.

  * Please make sure any of sequence aligners (clustalw2 | t\_coffee | mafft | muscle) is installed successfully, put into the global executable directory and thus can be accessible from any working directory. Otherwise specify its full path by parameter "-m, -msa".

  * Please make sure Epal2nal.pl is executable (+X) and put into the global directory.


  * If you would like to use [KaKs\_Calculator](http://code.google.com/p/kaks-calculator) to estimate Ka and Ks for the resulting alignments, please make sure:
    1. the outputted sequences should be in AXT format;
    1. [KaKs\_Calculator](http://code.google.com/p/kaks-calculator) is installed successfully, put into the global executable directory and thus can be accessible from any working directory.

# Parameter settings #

|-h, -homolog|Homolog group file [string, required]|
|:-----------|:------------------------------------|
|-a, -aminoacid|File containing multiple amino acid sequences [string, required]|
|-n, -nuc    |File containing multiple nucleotide sequences [string, required]|
|-p, -processor|File containing the number of processes, changeable during running [string, required]|
|-o, -output |Output folder [string, required]     |
|-m, -msa    |Multiple sequence aligner or specify with its full path (clustalw2 | t\_coffee | mafft | muscle, optional)|
|-f, -format |Output file format (axt | fasta | paml | codon | clustal, optional), default = fasta|
|-c, -code   |Genetic Code used [integer, optional], default = 1-The Standard Code|
|            |1-The Standard Code                  |
|            |2-The Vertebrate Mitochondrial Code  |
|            |3-The Yeast Mitochondrial Code       |
|            |4-The Mold, Protozoan, and Coelenterate Mitochondrial Code and the Mycoplasma/Spiroplasma Code|
|            |5-The Invertebrate Mitochondrial Code|
|            |6-The Ciliate, Dasycladacean and Hexamita Nuclear Code|
|            |9-The Echinoderm and Flatworm Mitochondrial Code|
|            |10-The Euplotid Nuclear Code         |
|            |11-The Bacterial, Archaeal and Plant Plastid Code|
|            |12-The Alternative Yeast Nuclear Code|
|            |13-The Ascidian Mitochondrial Code   |
|            |14-The Alternative Flatworm Mitochondrial Code|
|            |15-Blepharisma Nuclear Code          |
|            |16-Chlorophycean Mitochondrial Code  |
|            |21-Trematode Mitochondrial Code      |
|            |22-Scenedesmus Obliquus Mitochondrial Code|
|            |23-Thraustochytrium Mitochondrial Code|
|            |See all documented codes at [http://www.ncbi.nlm.nih.gov/Taxonomy/Utils/wprintgc.cgi](http://www.ncbi.nlm.nih.gov/Taxonomy/Utils/wprintgc.cgi).|
|-g, -nogap  |Remove aligned codons with gaps      |
|-t, -nomismatch|Remove mismatched codons             |
|-k, -kaks   |Enable using [KaKs\_Calculator](http://code.google.com/p/kaks-calculator) for Ka and Ks estimation (requiring axt format)|
|-v, -verbose|Verbose output                       |
|-?, -help   |Display help information             |

# Example #

Before running, please make sure ParaAT.pl, Epal2nal.pl and multiple sequence aligner (clustalw2 | t\_coffee | mafft | muscle) are put into the global directory and can be accessible from any working directory.

Supposing the following files.
|**File Name**|**Description**|
|:------------|:--------------|
|hmc.homolog  |9,712 human-mouse-chimp orthologous groups|
|hmc.pep      |A file containing all amino acids sequences for human, mouse, chimp|
|hmc.cds      |A file containing all nucleotide sequences  for human, mouse, chimp|
|hmc.proc     |A file containing a number, viz., the number of processor to be used|

Thus, the command to run ParaAT is below and the results are outputted into a folder named "hmc-output":
```
ParaAT.pl -homolog hmc.homolog -aminoacid hmc.pep -nuc hmc.cds -processor hmc.proc -output hmc-output
```

To output the axt-formatted sequences, the command is:
```
ParaAT.pl -homolog hmc.homolog -aminoacid hmc.pep -nuc hmc.cds -processor hmc.proc -output hmc-output -format axt
```

To output the axt-formatted sequences and perform Ka and Ks calculations, the command is:
```
ParaAT.pl -homolog hmc.homolog -aminoacid hmc.pep -nuc hmc.cds -processor hmc.proc -output hmc-output -format axt -kaks
```

The outputted sequences with AXT format are below.

```
NP_000006-NP_032699
ATGGACATTGAAGCATATTTTGAAAGAATTGGCTATAAGAACTCTAGGAACAAATTGGACTTGGAAACATTAACTGACATTCTTGAGCACCAGATCCGGGCTGTTCCCTTTGAGAACCTTAACATGCATTGTGGGCAAGCCATGGAGTTGGGCTTAGAGGCTATTTTTGATCACATTGTAAGAAGAAACCGGGGTGGGTGGTGTCTCCAGGTCAATCAACTTCTGTACTGGGCTCTGACCACAATCGGTTTTCAGACCACAATGTTAGGAGGGTATTTTTACATCCCTCCAGTTAACAAATACAGCACTGGCATGGTTCACCTTCTCCTGCAGGTGACCATTGACGGCAGGAATTACATTGTCGATGCTGGGTCTGGAAGCTCCTCCCAGATGTGGCAGCCTCTAGAATTAATTTCTGGGAAGGATCAGCCTCAGGTGCCTTGCATTTTCTGCTTGACAGAAGAGAGAGGAATCTGGTACCTGGACCAAATCAGGAGAGAGCAGTATATTACAAACAAAGAATTTCTTAATTCTCATCTCCTGCCAAAGAAGAAACACCAAAAAATATACTTATTTACGCTTGAACCTCGAACAATTGAAGATTTTGAGTCTATGAATACATACCTGCAGACGTCTCCAACATCTTCATTTATAACCACATCATTTTGTTCCTTGCAGACCCCAGAAGGGGTTTACTGTTTGGTGGGCTTCATCCTCACCTATAGAAAATTCAATTATAAAGACAATACAGATCTGGTCGAGTTTAAAACTCTCACTGAGGAAGAGGTTGAAGAAGTGCTGAGAAATATATTTAAGATTTCCTTGGGGAGAAATCTCGTGCCCAAACCTGGTGATGGATCCCTTACTATT
ATGGACATCGAAGCATACTTTGAAAGGATTGGTTACAAGAACTCAGTGAATAAATTGGACTTAGCCACATTAACTGAAGTTCTTCAGCACCAGATGCGAGCAGTTCCTTTTGAGAATCTTAACATGCATTGTGGAGAAGCCATGCATCTGGATTTACAGGACATTTTTGACCACATAGTAAGGAAGAAGAGAGGTGGATGGTGTCTCCAGGTTAATCATCTGCTGTACTGGGCTCTGACCAAAATGGGCTTTGAAACCACAATGTTGGGAGGATATGTTTACATAACTCCAGTCAGCAAATATAGCAGTGAAATGGTCCACCTTCTAGTACAGGTGACCATCAGTGACAGGAAGTACATTGTGGATTCCGCCTATGGAGGCTCCTACCAGATGTGGGAGCCTCTGGAATTAACATCTGGGAAGGATCAGCCTCAGGTGCCTGCCATCTTCCTTTTGACAGAGGAGAATGGAACCTGGTACTTGGACCAAATCAGAAGAGAGCAGTATGTTCCAAATGAAGAATTTGTTAACTCAGACCTCCTTGAAAAGAACAAATATCGAAAAATCTACTCCTTTACTCTTGAGCCCCGAGTTATCGAGGATTTTGAATATGTGAATAGCTATCTTCAGACATCGCCAGCATCTGTGTTTGTAAGCACATCGTTCTGTTCCTTGCAGACCTCGGAAGGGGTTCACTGTTTAGTGGGCTCCACCTTTACAAGTAGGAGATTCAGCTATAAGGACGATGTAGATCTGGTTGAGTTTAAATATGTGAATGAGGAAGAAATAGAAGATGTACTGAAAACCGCATTTGGCATTTCTTTGGAGAGAAAGTTTGTGCCCAAACATGGTGAACTAGTTTTTACTATT

NP_000008-NP_031409
ATGGCCGCCGCGCTGCTCGCCCGGGCCTCGGGCCCTGCCCGCAGAGCTCTCTGTCCTAGGGCCTGGCGGCAGTTACACACCATCTACCAGTCTGTGGAACTGCCCGAGACACACCAGATGTTGCTCCAGACATGCCGGGACTTTGCCGAGAAGGAGTTGTTTCCCATTGCAGCCCAGGTGGATAAGGAACATCTCTTCCCAGCGGCTCAGGTGAAGAAGATGGGCGGGCTTGGGCTTCTGGCCATGGACGTGCCCGAGGAGCTTGGCGGTGCTGGCCTCGATTACCTGGCCTACGCCATCGCCATGGAGGAGATCAGCCGTGGCTGCGCCTCCACCGGAGTCATCATGAGTGTCAACAACTCTCTCTACCTGGGGCCCATCTTGAAGTTTGGCTCCAAGGAGCAGAAGCAGGCGTGGGTCACGCCTTTCACCAGTGGTGACAAAATTGGCTGCTTTGCCCTCAGCGAACCAGGGAACGGCAGTGATGCAGGAGCTGCGTCCACCACCGCCCGGGCCGAGGGCGACTCATGGGTTCTGAATGGAACCAAAGCCTGGATCACCAATGCCTGGGAGGCTTCGGCTGCCGTGGTCTTTGCCAGCACGGACAGAGCCCTGCAAAACAAGGGCATCAGTGCCTTCCTGGTCCCCATGCCAACGCCTGGGCTCACGTTGGGGAAGAAAGAAGACAAGCTGGGCATCCGGGGCTCATCCACGGCCAACCTCATCTTTGAGGACTGTCGCATCCCCAAGGACAGCATCCTGGGGGAGCCAGGGATGGGCTTCAAGATAGCCATGCAAACCCTGGACATGGGCCGCATCGGCATCGCCTCCCAGGCCCTGGGCATTGCCCAGACCGCCCTCGATTGTGCTGTGAACTACGCTGAGAATCGCATGGCCTTCGGGGCGCCCCTCACCAAGCTCCAGGTCATCCAGTTCAAGTTGGCAGACATGGCCCTGGCCCTGGAGAGTGCCCGGCTGCTGACCTGGCGCGCTGCCATGCTGAAGGATAACAAGAAGCCTTTCATCAAGGAGGCAGCCATGGCCAAGCTGGCCGCCTCGGAGGCCGCGACCGCCATCAGCCACCAGGCCATCCAGATCCTGGGCGGCATGGGCTACGTGACAGAGATGCCGGCAGAGCGGCACTACCGCGACGCCCGCATCACTGAGATCTACGAGGGCACCAGCGAAATCCAGCGGCTGGTGATCGCCGGGCATCTGCTCAGGAGCTACCGGAGC
ATGGCTGCCGCCTTGCTCGCCCGGGCCCGTGGCCCTCTCCGTAGAGCTCTCGGTGTTCGGGACTGGCGACGGTTACACACTGTTTACCAGTCTGTGGAGCTGCCTGAGACACACCAGATGTTGCGTCAGACATGCCGTGACTTTGCCGAGAAGGAGTTGGTCCCCATTGCGGCCCAGCTGGACAGGGAGCATCTCTTCCCCACAGCTCAGGTTAAGAAGATGGGTGAGCTCGGGCTGCTGGCCATGGATGTGCCAGAGGAGCTGAGTGGTGCAGGCTTGGATTACCTGGCCTACTCCATCGCCCTGGAGGAGATCAGCCGTGCCTGCGCCTCCACGGGAGTTATCATGAGCGTCAACAATTCTCTCTACTTGGGACCCATTCTGAAGTTTGGATCCGCACAGCAGAAGCAACAGTGGATCACCCCTTTCACCAATGGTGACAAAATCGGCTGTTTTGCCCTCAGTGAGCCAGGCAATGGCAGTGATGCTGGAGCCGCTTCCACCACTGCCCGGGAAGAGGGTGACTCATGGGTCCTCAACGGCACCAAAGCTTGGATCACCAACTCCTGGGAGGCTTCCGCCACGGTGGTATTTGCCAGCACAGACAGGTCCCGGCAGAACAAGGGTATCAGTGCCTTCCTGGTTCCCATGCCAACTCCTGGGCTCACGCTGGGCAAGAAGGAAGACAAGCTGGGCATCCGGGCCTCCTCCACAGCTAACCTCATCTTTGAGGACTGCCGGATCCCCAAGGAGAACCTGCTTGGGGAGCCTGGGATGGGCTTCAAAATAGCCATGCAAACCCTGGACATGGGTCGCATTGGCATCGCCTCCCAGGCCCTGGGCATCGCCCAGGCCTCCCTGGATTGTGCTGTGAAGTATGCCGAGAACCGCAATGCCTTTGGGGCACCGCTCACCAAGCTCCAAAATATCCAGTTCAAGCTGGCAGACATGGCCCTGGCCCTGGAGAGTGCCCGCCTGCTGACCTGGCGTGCTGCCATGTTGAAAGACAACAAGAAACCTTTCACCAAGGAGTCCGCCATGGCCAAACTGGCTGCATCGGAGGCTGCAACCGCCATTAGCCACCAGGCCATCCAGATCCTGGGCGGCATGGGGTATGTGACAGAGATGCCGGCTGAGCGGTACTACCGAGATGCCCGCATCACTGAGATCTACGAAGGGACCAGCGAAATCCAGAGACTGGTGATCGCTGGGCATCTGCTCCGGAGCTACCGGAGC
```