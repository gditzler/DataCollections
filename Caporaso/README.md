# About 

The Caporaso et al. data was downloaded from the [Earth Microbiome Project](http://www.earthmicrobiome.org/) under the identifer [study 550](ftp://thebeast.colorado.edu/pub/QIIME_DB_Public_Studies/study_550_split_library_seqs_and_mapping.zip) (*warning - clicking the latter link will download a rather large file*). The data contains samples collected from two patients (one male, one female) on four different body sites. There are 396 different time points in the study. 

# Notes on Preprocessing 

QIIME was used to split the original biom and map file (`caporaso.biom` and `caporaso.txt`) by the body sites. The `AmericanGut` folder in this repository has some of the code that can be used to implement the splitting. 
