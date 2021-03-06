# lncrna.annotator
Positional annotation of lncRNAs in GTF file

Annotator
# A script to perform genomic positional annotation of lncRNAs.

# Version 5
# April 2014

# By Rory Johnson, University of Bern: rory.johnson@dbmr.unibe.ch or roryjohnson1@gmail.com or www.gold-lab.org

# Usage: sh lncrna.annotator.5.sh project_name lncRNA_annotation_gtf entire_annotation_gtf

####
#VERSION 2: change intergenic lncRNA-protcod distance estimation so now the reported distance is that given by BEDTOOLS, ie the nearest to nearest distance, NOT the promoter-promoter distance as previously.
####
#VERSION 3: change the commands that create the lncRNA and pc BED files (genes, transcripts, exons) to be based on perl pattern matching, so that they are not sensitive to the order that things are defined in the description column of the GTF, which can sometimes change.
####
#VERSION 5: follows from V3 (except fragment from V4, where indicated). Remove the extra V4 functionality but here fixing the problem with unstranded transcripts.
# Add new classification for unstranded trxs.
###


# Important points:

# Input files: Two separate GTF format files - (1) lncRNA annotation and (2) full annotation, both available from gencodegenes.org.
# For example: gencode.v19.long_noncoding_RNAs.gtf and gencode.v19.annotation.gtf.

# LncRNAs are treated at transcript level, protein-coding genes are treated at gene level.

# Output files are saved in a folder called: <<project_name>>.output

# Principal output file: <<project_name>>.output/<<project_name>>.classification.output
# Format: LncRNA_Transcript_ID, Nearest_protein_coding_gene_ID, Positional_annotation_class, Distance (bp) 

# How distances are reported:
# Distances (ONLY for genic lncRNAs): negative=lncRNA upstream of protcod, positive= lncRNA downsteram of protcod
# Lncrna-protcod distance is reported differently for intergenic or genic lncRNAs:
# Intergenic-distance of nearest points
# Genic-distance of promoters

# LncRNA types: 
#(0) unstranded, intergenic
#(1) samestrand, lincRNA upstream / 
#(2) divergent / 
#(3) samestrand, protcod upstream / 
#(4) convergent / 
#(5) intronic_AS / 
#(6) intronic_SS / 
#(7) exonic_AS / 
#(8) exonic_SS
#(9) unstranded, genic
# In cases where lncRNA can have two or more types, the higher-numbered type gets chosen.

# There is no minimum window size around protein coding genes.

# Calculate distances thus:
# lncRNA + strand, Protcod - strand: $2-$8
# -, -, $9-$3
# +, -, $9-$2
# -, +, $3-$8

# Note: lncRNA annotation must contain transcripts and exons, entire annotation must contain genes. Other features will be discarded.

# Note: if you need to create the lncRNA subannotation gtf file from the total file, use something like this command:
# cat gencode20_havana.gtf | awk '$14!~/protein|pseudo|TR_gene|IG_gene/' > gencode20_havana.lncRNA.gtf




