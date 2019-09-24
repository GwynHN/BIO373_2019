# Mapping reads to reference genome: DNA edition

Here we map reads to a reference genome. These correspond to DNA, whereas Masa did RNA mapping/alignment with you last week. They are similar in the types of steps to execute, but the underlying concepts are a bit different.

Before you map, you usually want to check the quality of the reads using something like `fastqc` and then you definitely want to trim the reads. No use in trying to map poor quality data. These steps have already been done, so we start with the mapping  

## Map reads with BWA

**Make sure you're back in your VarCall/ directory**

First, set up the environment with the software we will use. BWA and samtools. In order to run the analysis, we need to index the reference sequence

    $ module load Aligner/BWA/0.7.15
    $ module load Tools/samtools/1.9
    $ bwa index 00_input/MedtrChr2.fa

Now we can run the mapping tool BWA and pipe the output through samtools, which will sort the output so we can use it in subsequent steps.

    $ mkdir 01_aligned
    $ acc=w516950
    $ bwa mem -M -t 2 -R "@RG\tID:CAV90ANXX.6\tPL:Illumina\tLB:${acc}\tSM:${acc}" 00_input/MedtrChr2.fa 00_input/${acc}_R{1,2}.fastq.gz | samtools sort -m 16G -T /scratch/gwynhn -o 01_aligned/${acc}.sorted.bam


