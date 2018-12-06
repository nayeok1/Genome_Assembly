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

#### Module load

     $ module load jje/jjeutils perl rstudio/0.99.9.9

#### Plots for whole genome

      
         $ bioawk -c fastx ' { print length($seq) } ' dmel-all-chromosome-r6.24.fasta | sort -rn | awk ' BEGIN { print "Assembly\tLength\nseq_length\t0" } { print "seq_length\t" $1 } ' > length-whole-genome.length # sequence length
         $ plotCDF2 length-whole-genome.length length-whole-genome.png # output graph
         $ bioawk -c fastx '{ print $name, gc($seq) }' dmel-all-chromosome-r6.24.fasta | sort -rn | awk ' BEGIN { print "Assembly\tLength\nkgc_Ctg\t0" } { print "kbgc_Ctg\t" $1 } ' >  gc-whole-genome-lengths #GC percentage
         $ plotCDF2 gc-whole-genome-lengths gc-whole-genome.png # output graph 
         
1. Sequence length distribution   

![len-whole](https://blogfiles.pstatic.net/MjAxODEyMDZfMjMx/MDAxNTQ0MDk2NDA5NjIx.dYbVr_AdfEXVGTCAO-t4T8N7p1Zjqttdrzm9SptmL6Ig.-BELOSgyJzbYZtHz7KGJvXxK93OADlm_55ro2tnSTH0g.PNG.nayeonkim93/length-whole-genome.png)
2. Sequence GC% distribution   
3. Cumulative genome size sorted from largest to smallest sequences   

#### Plots for sequence ≤ 100kb

         $ bioawk -c fastx ' { print length($seq) } ' dmel-all-chromosome-r6.24-lessandequal100kb.fasta | sort -rn | awk ' BEGIN { print "Assembly\tLength\nseq_length\t0" } { print "seq_length\t" $1 } ' > length-less-genome.length 
         $ plotCDF2 length-less-genome.length length-less-genome.png 
         $ bioawk -c fastx '{ print $name, gc($seq) }' dmel-all-chromosome-r6.24-lessandequal100kb.fasta | sort -rn | awk ' BEGIN { print "Assembly\tLength\nkgc_Ctg\t0" } { print "kbgc_Ctg\t" $1 } ' >  gc-less-genome-lengths 
         $ plotCDF2 gc-less-genome-lengths gc-less-genome.png 
         
1. Sequence length distribution   

![len-less](https://blogfiles.pstatic.net/MjAxODEyMDZfMTIz/MDAxNTQ0MDk2NDE1OTc1.wgP88M7T4ml8YY9kOQf7Kdan9EAsRma3Ob_IVxyzO7kg.W6tvCuNmKciW2upqL0UeNG0tAERO_PIs-lxgjT4KTqcg.PNG.nayeonkim93/length-less-genome.png)
2. Sequence GC% distribution   
3. Cumulative genome size sorted from largest to smallest sequences   

#### Plots for sequence > 100kb

         $ bioawk -c fastx ' { print length($seq) } ' dmel-all-chromosome-r6.24-greater100kb.fasta | sort -rn | awk ' BEGIN { print "Assembly\tLength\nseq_length\t0" } { print "seq_length\t" $1 } ' > length-great-genome.length
         $ plotCDF2 length-great-genome.length length-great-genome.png
         $ bioawk -c fastx '{ print $name, gc($seq) }' dmel-all-chromosome-r6.24-greater100kb.fasta | sort -rn | awk ' BEGIN { print "Assembly\tLength\nkgc_Ctg\t0" } { print "kbgc_Ctg\t" $1 } ' >  gc-great-genome-lengths
         $ plotCDF2 gc-great-genome-lengths gc-great-genome.png
 
1. Sequence length distribution    

![len-great](https://blogfiles.pstatic.net/MjAxODEyMDZfMTI0/MDAxNTQ0MDk2NDIyNzk5.Vo5bukEg4yEmMbUl4Nj-TbPIzpZjW8ODyWV-ontO3O0g.hTDhCAvZEppVJodRyZd9sg8o_ELKCaaKU99m4m3L3eIg.PNG.nayeonkim93/length-great-genome.png)
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
        $ [M::main] ===> Step 1: reading read mappings <===
          [M::ma_hit_read::0.001*1.74] read 0 hits; stored 0 hits and 0 sequences (0 bp)
          [M::main] ===> Step 2: 1-pass (crude) read selection <===
          [M::ma_hit_sub::0.001*1.31] 0 query sequences remain after sub
          [M::ma_hit_cut::0.001*1.19] 0 hits remain after cut
          [M::ma_hit_flt::0.001*1.07] 0 hits remain after filtering; crude coverage after filtering: -nan
          [M::main] ===> Step 3: 2-pass (fine) read selection <===
          [M::ma_hit_sub::0.001*0.91] 0 query sequences remain after sub
          [M::ma_hit_cut::0.001*0.84] 0 hits remain after cut
          [M::ma_hit_contained::0.001*0.79] 0 sequences and 0 hits remain after containment removal
          [M::main] ===> Step 4: graph cleaning <===
          [M::ma_sg_gen] read 0 arcs
          [M::main] ===> Step 4.1: transitive reduction <===
          [M::asg_arc_del_trans] transitively reduced 0 arcs
          [M::main] ===> Step 4.2: initial tip cutting and bubble popping <===
          [M::asg_cut_tip] cut 0 tips
          [M::asg_arc_del_multi] removed 0 multi-arcs
          [M::asg_arc_del_asymm] removed 0 asymmetric arcs
          [M::asg_pop_bubble] popped 0 bubbles and trimmed 0 tips
          [M::main] ===> Step 4.3: cutting short overlaps (3 rounds in total) <===
          [M::asg_arc_del_short] removed 0 short overlaps
          [M::asg_arc_del_short] removed 0 short overlaps
          [M::asg_arc_del_short] removed 0 short overlaps
          [M::main] ===> Step 4.4: removing short internal sequences and bi-loops <===
          [M::asg_cut_internal] cut 0 internal sequences
          [M::asg_cut_biloop] cut 0 small bi-loops
          [M::asg_cut_tip] cut 0 tips
          [M::asg_pop_bubble] popped 0 bubbles and trimmed 0 tips
          [M::main] ===> Step 4.5: aggressively cutting short overlaps <===
          [M::asg_arc_del_short] removed 0 short overlaps
          [M::main] ===> Step 5: generating unitigs <===
          [M::main] Version: 0.2-r168-dirty
          [M::main] CMD: miniasm -f reads.fq reads.paf.gz
          [M::main] Real time: 0.003 sec; CPU: 0.001 sec
### Assembly assessment

1. Calculate the N50 of your assembly (this can be done with only faSize+awk+sort or with bioawk+awk+sort) and compare it to the Drosophila community reference's contig N50   
                  
          
          
2. Compare your assembly to the contig assembly (not the scaffold assembly!) from Drosophila melanogaster on FlyBase using a dotplot constructed with MUMmer   

#### Module load and make contig assemby using faSplitByN

          $ module load jje/jjeutils perl
          $ faSplitByN dmel-all-chromosome-r6.24.fasta dmel-all-chromosome-r6.24-contass.fasta
         
#### From Edwin note

          $ # Loading of binaries via module load or PATH reassignment
            source /pub/jje/ee282/bin/.qmbashrc
            module load gnuplot

            # Query and Reference Assignment. State my prefix for output filenames
            REF="dmel-all-chromosome-r6.24-contass.fasta"
            PREFIX="flybase"
            SGE_TASK_ID=1
            QRY=$(ls u*.fa | head -n $SGE_TASK_ID | tail -n 1)
            PREFIX=${PREFIX}_$(basename ${QRY} .fa)

            # Please use a value between 75-150 for -c. The value of 1000 is too strict.
            nucmer -l 100 -c 150 -d 10 -banded -D 5 -prefix ${PREFIX} ${REF} ${QRY}
            mummerplot --fat --layout --filter -p ${PREFIX} ${PREFIX}.delta \
            -R ${REF} -Q ${QRY} --postscript

          

3. Compare your assembly to both the contig assembly and the scaffold assembly from the Drosophila melanogaster on FlyBase using a contiguity plot   
4. Calculate BUSCO scores of both assemblies and compare them   
