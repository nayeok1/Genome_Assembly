# Homework 4

## Summarize partitions of a genome assembly

We will be revisiting the Drosophila melanogaster genome. As with Homework 3, start at flybase.org. Go to the most current download genomes section and download the gzipped fasta file for all chromosomes.

### Calculate the following for all sequences ≤ 100kb and all sequences > 100kb:

#### Preparation of Data

      $ cd HW4 #change directory to HW4   
      $ wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/fasta/dmel-all-chromosome-r6.24.fasta.gz #downloading data   
      $ md5sum dmel-all-chromosome-r6.24.fasta.gz #checking the file integrity   
      $ gunzip dmel-all-chromosome-r6.24.fasta.gz #unzipping the file

#### Module load

      $ module load jje/jjeutils jje/kent #need this for bioawk and faSize

I am going to use the bioawk for partitioning. 

#### Partition using BIOAWK
      $ bioawk -c fastx 'length($seq)  <= 100000{ print ">"$name; print $seq }' dmel-all-chromosome-r6.24.fasta | sort -rn > dmel-all-chromosome-r6.24-lessandequal100kb.fasta #file save with -lessandequal100kb
      $ bioawk -c fastx 'length($seq)  > 100000{ print ">"$name; print $seq }' dmel-all-chromosome-r6.24.fasta | sort -rn > dmel-all-chromosome-r6.24-greater100kb.fasta #file saved with -greater100kb
   
#### Checking the file in HW4 directory
      $ ls 
      $ dmel-all-chromosome-r6.24.fasta                    README.md
        dmel-all-chromosome-r6.24-greater100kb.fasta       
        dmel-all-chromosome-r6.24-lessandequal100kb.fasta

Now I know that I have successfully saved two files with sequence ≤ 100kb and >100kb. We are going to use faSize to get total number of nucleotides, total number of Ns and total number of sequences

#### Getting data using FASIZE
      $ faSize dmel-all-chromosome-r6.24-lessandequal100kb.fasta 
      $ 6178042 bases (662593 N's 5515449 real 5515449 upper 0 lower) in 1863 sequences in 1 files
        Total size: mean 3316.2 sd 108053.8 min 0 (211000022278031) max 4245830 (mitochondrion_genome) median 0
        N count: mean 355.7 sd 11351.2
        U count: mean 2960.5 sd 96726.1
        L count: mean 0.0 sd 0.0
        %0.00 masked total, %0.00 masked real
        
      $ faSize dmel-all-chromosome-r6.24-greater100kb.fasta  
      $ 137547960 bases (490385 N's 137057575 real 137057575 upper 0 lower) in 7 sequences in 1 files 
        Total size: mean 19649708.6 sd 51988242.2 min 0 (2L) max 137547960 (X) median 0
        N count: mean 70055.0 sd 185348.1
        U count: mean 19579653.6 sd 51802894.1
        L count: mean 0.0 sd 0.0
        %0.00 masked total, %0.00 masked real

##### Data from sequence ≤ 100kb
1. Total number of nucleotides : 6178042 nucleotides
2. Total number of Ns : 662593 N's
3. Total number of sequences : 1863 sequences

##### Data from sequence > 100kb
1. Total number of nucleotides : 137547960 nucleotides
2. Total number of Ns : 490385 N's
3. Total number of sequences : 7 sequences


### Plots of the following for the whole genome, for all sequences ≤ 100kb, and all sequences > 100kb:

1. Sequence length distribution
2. Sequence GC% distribution
3. Cumulative genome size sorted from largest to smallest sequences


## Genome assembly

### Assemble a genome from MinION reads

1. Download the reads from here   

        $ cd HW4
        # mkdir assembly
        # cd assembly
        $ wget https://hpc.oit.uci.edu/~solarese/ee282/iso1_onp_a2_1kb.fastq.gz #downloading data
        $ ln -s /data/users/nayeok1/HW4/assembly/iso1_onp_a2_1kb.fastq reads.fq
        
2. Use minimap to overlap reads     

        $ minimap -x ava-pb -t8 reads.fq reads.fq | gzip -1 > reads.paf.gz
3. Use miniasm to construct an assembly   

        $ miniasm -f reads.fq reads.paf.gz > reads.gfa
### Assembly assessment

1. Calculate the N50 of your assembly (this can be done with only faSize+awk+sort or with bioawk+awk+sort) and compare it to the Drosophila community reference's contig N50   
2. Compare your assembly to the contig assembly (not the scaffold assembly!) from Drosophila melanogaster on FlyBase using a dotplot constructed with MUMmer   
3. Compare your assembly to both the contig assembly and the scaffold assembly from the Drosophila melanogaster on FlyBase using a contiguity plot   
4. Calculate BUSCO scores of both assemblies and compare them   
