# APERO
small RNA detection from paired-end RNA-seq data

# Program principle
APERO is written in R and can be used on all operating systems after installing several R packages (Rsamtools, Reshape2, Snowfall). A webserver version is also available on a local instance of the Galaxy bioinformatics platform (bioinfo.insa-lyon.fr), which provides an installation-free access to all users. The workflow of the program is described in Fig. 1. The required input is a BAM sequence alignment file from a RNA-Seq dataset, and the output is a text file giving the list of detected sRNA, with different information fields (genomic coordinates, strand, value of the width parameter used, intensity of sRNA start and number of iterations of the second module). Optionally, the user can provide a genome annotation (in ptt format), which is then used to classify each sRNA with respect to neighboring or overlapping CDS.

 APERO is composed of two separate modules. The first one is dedicated to the identification and mapping of transcriptional start sites (TSSs), i.e., the 5’ ends of putative sRNAs. Inspired by previous algorithms specifically dedicated to TSS detection, TSSs are defined as regions exhibiting a local enrichment in read starts compared to neighbor regions. Two common difficulties are encountered: (1) due to wobbling of the RNA polymerase, TSSs peaks are not always defined sharply, but can extend over several nucleotides; (2) adjacent peaks distant of several nucleotides are sometimes observed, reflecting alternate 5’ starts of the same transcript which prevent the identification of individual peaks. The analysis procedure was designed to handle these issues and optimize precision, by varying the scanning window sizes and favoring the most well-defined TSSs. The user can play on two parameters : the maximal accepted width of a TSS peak (wmax), and the minimal distance between separate TSSs (i.e., the spatial resolution of the detection method, d). An advantage of our method compared to previous ones is that it can be applied either on usual RNA-Seq datasets or on RNAs treated with Terminal Exonuclease (TEX) for primary transcript enrichment. In the latter case, the transcript starts are found with increased precision; in both cases, the precision is better than that of previous sRNA detection algorithms based on read coverage. 
 
The second module of APERO starts from the identified TSSs, and localizes the 3’ end of each sRNA. Theoretically, since the small transcripts were not fragmented, the paired-end sequencing should immediately allow identifying the 3’ boundary from the aligned reads. However, due to experimental bias, spontaneous fragmentation and degradation of 3’-ends by polynucleotide phosphorylase, sRNA lengths are generally underestimated by this approach. Starting from an identified TSS, the method thus consists in iteratively extending the identified sRNA by locating the 3’ end of the longest fragments (if their number is statistically significant). This end position can then be either the actual 3’ end of the transcript, or a mere intermediate point or spontaneous breaking site within the transcript; to distinguish between these two scenarios, the program computes the number of fragments that overlap this position and were not yet counted in the previous iteration. If this number is significant (as compared to the number of fragments starting at the 5’ end, i.e. the expression strength of this sRNA), the sRNA is further extended toward the 3’ end of these overlapping fragments and the operation is repeated; otherwise, the iteration stops and this point is defined as the 3’ end.

# Installation 
The package require the Rsamtools, reshape2 and snowfall libraries. Please install these libraries before runing APERO

1- Download the APERO packge (.tar.gz file)

2- Open R

3- Install the APERO package : install.packages("path/to/APERO/package.tar.gz", type="source",repos=NULL)


# Usage
After installation, load the package with : library(APERO)

APERO package contain two functions; One for each module. 
Please refer to the help functions with help("APERO_start_detection") and help("APERO_stop_detection") for further documentation

# Reference
If you want two know more about APERO program and evaluation, have a look a the APERO publication (Leonard et al., in preparation)


# Contact
Don't hesitate to contact me for any question at simon_leonard[a]hotmail[.]fr
