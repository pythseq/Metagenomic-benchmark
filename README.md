**Metagenomic-benchmark**

The code deposited in this site contains the necessary scripts to integrate, standardise and generate taxonomic annotation matrices at the eight basic levels of taxonomic lineage: domain, phylum, class, order, family, genus, species and subspecies. These tools provide the possibility of comparing taxonomic annotation results obtained from different methods and databases. The available scripts were designed to manage the results obtained from the following taxonomic annotation tools:

- Kraken v0.10.5-beta
- CLARK v1.2.3.1
- MetaPhlAn2 v2.2.0 
- MOCAT v1.3
- QIIME v1.9.1
- SPINGO v1.3
- Metaxa2 v2.1.1
- Parallel-meta v2.4.1 and v3.3.2


Taxonomic databases for 16S rRNA ampicon annotation:

- SILVA v128
- GreenGenes v13.5
- RDP v11.5
- Metaxa2 database


**Requirements:**

Download and install the ete3 python library from:
http://etetoolkit.org/download/

Perl v5.x or higher
Python v2.7.x

**Patching Parallel-meta v2.4.1 **

To modify the parallel-meta v2.4.1 in order to accept the Metaxa2 database do the following:

-Before compiling the software, copy the patch_parallel241.txt file to the parallel-meta v2.4.1 root directory and run the command:

*patch -p0 < patch_parallel241.txt*

**This project includes the following scripts in the bin directory:**
- patch_parallel241.txt   - Patch for Parallel-meta v2.4.1
- count2percent.pl        - Normalize counts matrix into relative abundance
- shuffled_fasta.pl       - Generates shuffled sequences from a reference fasta file

Ete3 scripts:
- names2linaje.py
- names2taxid.py
- taxid2level.py
- taxid2linaje.py
- taxid2names.py
- lineage_ete.py
- translator_ete.py

Extra tables:
- ncbi_plus_unicos.txt     - Relational table containing the reference lineage taxids at the eight main taxonomic levels (superkingdom, phylum, class, order, family, genus, species, subespecies) for all the genomes used to generate the in-silico sequences.


Specifications about the software and parameters used are in the following table:

| Software | Parameters used |
|----------|-----------------|
| Metaxa2 v2.2.1 | metaxa2 -1 file_R1.fq -2 file_R2.fq -o metaxa_out -f fastq --plus T --fasta F --cpu 32 -d /PATH/database.fasta |
| Parallel-meta v2.4.1 | parallel-meta -b B -r\|m file.fastq -d database -n 32 |
| SPINGO v1.3 | spingo -p 32 -d /PATH/database.fasta-i file.fasta -a > result.txt |
| QIIME v1.9.1 | pick_otus.py -i file.fasta -o uclust_picked_otus --threads 16 <br/> pick_rep_set.py -i uclust_picked_otus/file_otus.txt -f file.fasta -l rep_set/file_rep_set.log -o rep_set/file_otus_rep_set.fasta <br/> assign_taxonomy.py -i rep_set/file_otus_rep_set.fasta -o uclust_assigned_taxonomy -r db.fasta -t taxonomy.txt <br/> make_otu_table.py -i uclust_picked_otus/file_otus.txt -t uclust_assigned_taxonomy/file_otus_rep_set_tax_assignments.txt -o otu_table.biom <br/> summarize_taxa.py -i otu_table.biom -o taxa_counts -a --suppress_biom_table_output |
| MetaPhlAn2 v2.2.0 | metaphlan2.py file_1.fq,file_2.fq --mpa_pkl mpa_v20_m200.pkl --bowtie2db mpa_v20_m200 --bt2_ps sensitive-local --bowtie2out file.bowtie2.bz2 --samout file.sam --nproc 32 --input_type fastq --min_alignment_len 95 > metaphlan_out.txt |
| MOCAT v1.3 | MOCAT.pl -sf samples.txt -rtf <br/> MOCAT.pl -sf samples.txt -s mOTU.v1.padded -r reads.processed -identity 97 <br/> MOCAT.pl -sf samples.txt -f mOTU.v1.padded -r reads.processed -identity 97 <br/> MOCAT.pl -sf samples.txt -p mOTU.v1.padded -r reads.processed -identity 97 -mode mOTU -o Results <br/> MOCAT.pl -sf samples.txt -s RefMG.v1.padded -r mOTU.v1.padded -e -identity 97 <br/> MOCAT.pl -sf samples.txt -f RefMG.v1.padded -r mOTU.v1.padded -e -identity 97 <br/> MOCAT.pl -sf samples.txt -p RefMG.v1.padded -r mOTU.v1.padded -e -identity 97 -mode RefMG -previous_db_calc_tax_stats_file -o Results |
| Kraken v0.10.5-beta | kraken --preload --db /Databases/Kraken/ --threads 16 --output kraken.out file.fastq <br/> kraken-report --db /Databases/Kraken/ kraken.out > kraken.report |
| CLARK v1.2.3.1 | classify_metagenome.sh -k 21 -P file_R1.fq file_R2.fq -R /PATH/clark.results -n 32 -m 0 |


All results were reported and analyzed in the submitted paper:

Escobar-Zepeda Alejandra, Godoy-Lozano E. Ernestina, Raggi Luciana, Segovia Lorenzo, Merino Enrique, Gutiérrez-Rios Rosa María, Juarez-Lopez Katy, Licea-Navarro Alexei F., Pardo-Lopez Liliana and Sanchez-Flores Alejandro. **Analysis of sequencing strategies and tools for taxonomic annotation: Defining standards for progressive metagenomics**. *submitted.* 2018.



