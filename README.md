ARGs_OAP manual
===============

The change log of this version (2019.12.07) includes:
1. database modification: 
Added polymyxin resistance genotypes mcr-1,mcr-1.2,mcr-1.3,mcr-1.4,mcr-1.5,mcr-1.6,mcr-1.7,mcr-1.8,mcr-1.9,mcr-2.1,mcr-2.2,mcr-3,mcr-4,mcr-5,eptA,icr-Mo,icr-Mc
Added sulfonamide resistance genotypes sul4 
Added quinolone resistance genotypes abaQ 
Added beta-lactam resistance genotypes NDM-1,NDM-9,NDM-10,NDM-11,NDM-12,NDM-13,NDM-16,NDM-14,NDM-15,NDM-17,NDM-20,NDM-18,NDM-19,vim-48,OXA-232 
Added multidrug resistance genotype qacEdelta1 
tetracycline resistance genotypes tetK,tetX,tetX1,tetX2,tetX3,tetX4
2. pipeline modification:
remove usearch (users need to download usearch 32 bit by themselves and move it into the folder "bin".)(if users have their own usearch bit64, a suitable SARG.2.2.udb and gg85.udb can be generated follow two steps:firstly, retrieve fasta “usearch_32bit -udb2fasta db.udb -output db.fasta", secondly, generate udb "usearch_64bit -makeudb_ublast db.fasta -output db.udb".)However, we are working in progress to replace usearch by minimap2 and diamond. A follow up update is coming soon after parameter optimization and validation.
3. pipeline modification 2:
The calculation is copy of ARGs divided by copies of 16S
The copy of ARGs has been changed to Number-of-ARG- like-sequence  × Length-of-hit-length / Length-of-ARG-reference-sequence 
The previous calculation method of copy of ARGs was Number-of-ARG- like-sequence  × Length-of-reads (i.e. 100 or 150) / Length-of-ARG-reference-sequence 

**Due to the requirement of usearch developer, we removed the binary usearch from the github, please download the usearch by yourself and rebuild the [gg85.udb](https://gg-sg-web.s3-us-west-2.amazonaws.com/downloads/greengenes_database/gg_13_5/gg_13_5_otus.tar.gz) from and SARGV2.0.udb and replace the two files under DB directory**

"usearch -makeudb_ublast gg85.fasta -output gg85.udb"

**Usful links:**

1) **A step by step video tutorial (less than 10 min) can be accessed from: https://www.youtube.com/watch?v=PCr1ctXvZPk**
2) **Online analysis platform for stage two: http://smile.hku.hk/SARGs**
3) **A mirror site was added in Shenzhen China for mainland China users to solve the slow data uploading problem [SUSTC-MIRROR-ARGS-OAP]( http://smile.sustc.edu.cn )**
  

**Novel features of version2.0**

- ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `1. The SARG database was expanded about three times in the version 2.0`  
- ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `2. SARGfam was supplied`    
- ![#1589F0](https://placehold.it/15/1589F0/000000?text=+)  `3. Cell number estimation with universial single-copy marker gene was added`    
- ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `4. Add -q for quality control of raw fastq files`    
- ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `5. Local version of stage two script`    
- ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `6. Adding option -s for very big input data (useful for users without 64bit usearch to split big input automatically)`      


**The DATABASE FILE of [SARG2.0](https://github.com/biofuture/Ublastx_stageone/blob/master/DB/SARG_20170328_12307.fasta) and the [structure](https://github.com/biofuture/Ublastx_stageone/blob/master/DB/structure_20170328_12037.LIST) of the database are under the DB directory.**

What does ARGs_OAP do:  
=====================

1. Fast environmental searching of antibiotic resistant gene in multiple metagenomics data sets; the ARGs abundance can be normalized to cell number
2. Generate mother table of type and sub-type level ARGs of users' samples and a merged sub-type level mother table    
3. Generate a PcoA of users samples with other typical environment samples such as human gut, ocean and sediment to show the relationship of user concerned samples with already sequenced environment.  

clone source code into local computer
=====================================
    git clone  https://github.com/biofuture/Ublastx_stageone.git  

Prepare the meta-data file of your samples  
==========================================
To run the stage one pipeline, users need to prepare relative meta-data.txt file and put all the pair-end fastq file into one directory  
Example of meta-data file **meta-data.txt**  Tips:   
* You need keep the first and second column's name as SampleID and Name
* The SampleID are required to be numbers counting from 1 to 2 to 3 etc.
* Category is the classification of your samples into groups and we will colored your samples in PcoA by this informaton
* The meta-data table should be separated by tabular for each of the items 
* The Name of each sample should be the fastq file names for your pair-end Illumina sequencing data, your fastq files will automatically be recognized by Name_1.fq and Name_2.fq, so you need to keep the name consistent with your fq file name. (if you files are end with .fastq or .fasta, you need to change them to end with .fq or .fa)
 
**Please make sure the meta-data file is pure txt format, if you edit the file under windows, using nodepad++ and check the end of each line by cliking View-> Show Symbol -> Show All Characters. If the line is end up with CRLF, please remove the CR by replace \r to nothing in the replace dialogue frame**

SampleID | Name | Category 
---------|------|-----------
 1       | STAS | ST    
 2       | SWHAS104 | SWH  

Prepare database and usearch
============================

SARG Database and 32 bit usearch is avaliable in DB/ and bin/ directory, respectively. **Users donot need to download CARD and ARDB anymore!!**

Stage one ARG_OAP
==================
When meta-data.txt and database files are prepared, then put all your fastq files into one directory in your local system (notice the name of your fastq files should be Name_1.fq and Name_2.fq). your can give -h to show the help information. Examples could be found in source directory example, in example directory run test:   

`nohup   ./argoap_pipeline_stageone_version2 -i inputfqs -o testoutdir -m meta-data.txt -n 8`

    ./argoap_pipeline_stageone_version2  -h
        Author: JIANG Xiaotao
        Modidied : 09-04-2018
        Email: biofuture.jiang@gmail.com
        ./argoap_pipeline_stageone_version2 -i <Fq input dir> -m <Metadata_map.txt> -o <output dir> -n [number of threads] -f [fa|fq] -z -h -c [S|U]

        -i Input files directory, required
        -m meta data file, required
        -o Output files directory, default current directory
        -n number of threads used for usearch, default 1
        -f the format of processed files, default fq
        -q quality control of fastq sequences defualt not take effect, set to 1, then will do QC with fastp
        -z whether the fq files were .gz format, if -z, then firstly gzip -d, default(none) 
        -x evalue for searching 16S in usearch default 1e-10
        -y evalue for searching universal single copy marker gene default 3
        -v the identity value for diamond to search the USCMGs default  0.45
        -s if fastq is too large, split into smaller ones 
        -c This option is to chose methods for estimating the prokyarto cell number  using copy number correction by Copywriter database to transfrom 16S information into cell number [ direct searching hyper variable region database by usearch] opetin "S", or using 30 universal single copy marker genes of prokaryote averagely coverage to estimate the cell number (Here, we ignore other eukaryota sequences in one metagenomics sample) represented by "U", (default U)
        -h print this help information


Stage one will search reads against SARG databbase and 16S greengene non-redundant 85 OTUs database to identify potential ARG reads and 16S reads. This step will generate searching results files for each fastq. This step also obtain the cell number of each fastq by searching essential single copy marker genes (version 2.0) or by searching against hyper-variable region database, and then perform copy number correction using Copyrighter copy number database (release date) to finally estimate the cell number of samples (version 1.0).
 
The results are in testoutdir/, it looks like this:

    extracted.fa                  STAS_2.16s                        SWHAS104.16s_hyperout.txt
    meta_data_online.txt          STAS_2.us                         SWHAS104_1.us
    STAS_1.16s                    STAS.extract_1.fa                 SWHAS104_2.16s
    STAS.16s_1v6.us               STAS.extract_2.fa                 SWHAS104_2.us
    STAS.16s_2v6.us               SWHAS104_1.16s                    SWHAS104.extract_1.fa
    STAS.16s_hvr_community.txt    SWHAS104.16s_1v6.us               SWHAS104.extract_2.fa
    STAS.16s_hvr_normal.copy.txt  SWHAS104.16s_2v6.us               ublastx_bash_Mon-Feb-1-16:20:59-2016.sh
    STAS.16s_hyperout.txt         SWHAS104.16s_hvr_community.txt
    STAS_1.us                     SWHAS104.16s_hvr_normal.copy.txt

The **extracted.fa** and **meta_data_online.txt** are two files needed for ublastx_stage_two analysis. The STAS.16s_hvr_community.txt is the microbial community of sample STAS and STAS.16s_hvr_normal.copy.txt is the averagely copy number of the microbial community after CopyRighter database correction. **In the version2, by default we use the universal single-copy gene to calculate the cell numbers rather than use 16S hyper variable region, hence, there will be no intermediate files like community.txt or copy.txt**   

The meta-data-online.txt looks like this 

SampleID | Name | Category | #ofreads | #of16S| **#ofCell**
---------|------|-----------|----------|-------|-------- 
 1       | STAS | ST  |200000 | 10.1  |   4.9
 2       | SWHAS104 | SWH  |200000 | 9.7 |    4.1


Stage two ARG_OAP for local runnig
========================================================
As many users have very big data, we supply a local version for the stage two analyses in this updation.   

**Prerequest for stage two script to run:**     
    1. download the whole fold of this repo.    
    2. install R packages **vegan, labdsv, ggplot2 and scales**  (Enter R and use install.packages(pkgs="vegan") to install these packages).       


    perl argoap_pipeline_stagetwo_version2
        Author: JIANG Xiao-Tao
        Modidied : 06-04-2018
        Email: biofuture.jiang@gmail.com
        ./argoap_pipeline_stagetwo_version2 -i <extracted.fa> -m <meta_data_online.txt> -n [number of threads] -l [length] -e [evalue] -d [identity] -o <output_prefix>
        
        -i the potential arg reads from stage one 
        -m meta data online from stage one 
        -o Output prefix
        -n number of threads used for blastx, default 1
        -l length filtering default 25 aa 
        -e evalue filtering default 1e-7
        -d identity filtering default 80
        -b indicate starting points/ if set then process the blastx results directly [default off]
        -h print this help information


We added a -b option for the stage two script if users already have the blastx outfmt 6 format resutls by alighment against SARG2.0.    
    **A typical scene is that users can paralelly run the blastx on clusters by multi-nodes, and then merge the blastx output as the input for the -b option.**
    
    perl argoap_pipeline_stagetwo_version2 -i extracted.fa -m meta_data_online.txt -o testout -b merge_blastx.out.txt 
   
Stage two pipeline on Galaxy system and download results
========================================================
Go to http://smile.hku.hk/SARGs  and using the module ARG_OAP.  

1. Using **ARG_OAP** -> **Upload Files** module to upload the extracted fasta file and meta_data_online.txt file generated in stage one into Galaxy  
2. Click **ARG_OAP** and **Ublast_stagetwo**, select your uploaded files  
3. For \"Column in Metadata:\" chose the column you want to classify your samples (default: 3)

Click **Execute** and you can find four output files for your information

After a while or so, you will notice that their are four files generated for your information.  
 
**File 1 and 2**: PcoA figures of your samples and other environment samples generated by ARGs abundance matrix normalization to 16s reads number and cell number  
**File 3 and 4**: Other tabular mother tables which including the profile of ARGs type and sub type information, as long as with other environment samples mother table. File3 results of ARGs abundance normalization aganist 16S reads number; File 4 results of ARGs abundance normalization aganist cell number


Repo of ARGs_OAP1.0 [Version 1.0](https://github.com/biofuture/Ublastx_stageone/releases/tag/Ublastx_stageone) 

**1. adding a method to obtain microbial community structure from the shotgun metagenomics data set.**  
**2. adding copy number correction using Copyrighter database and normalize ARGs abundance by cell number.**  

Detail introduction of copy number correction can be referred to [Transform ARGs abundance against cell number](https://github.com/biofuture/Ublastx_stageone/wiki/Transform-ARGs-abundance-against-cell-number)

There are some questions raised by users, please refer to the [FAQ](https://github.com/biofuture/Ublastx_stageone/wiki/FAQ) for details.  To run ARG OAP locally, users should download the source code into local computer system (Unix/Linux). Users can upload the generated files for stage two onto our Galaxy analysis platform (http://smile.hku.hk/SARGs) or use the local version of stage two script. 

------------------------------------------------------------------------------------------------------------------------  
**Notice:**

This tools only provide the required scripts for ARGs-OAP1.0/2.0 pipeline

This pipeline is distributed in the hope to achieve the aim of management of antibiotic resistant genes in envrionment, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.This pipeline is only allowed to be used for non-commercial and academic purpose.

**The SARG database is distributed only freely used for academic prupose, any commercial use should require the agreement from the developer team.** 



The copyrights of the following tools/databases which could be used in this pipeline belong to their original developers. The user of this pipeline should follow the guideline and regulations of these tools/database which could be found at the websites of their developers.  


1)     Usearch: (http://www.drive5.com/usearch/)

2)     Copyrighter: (http://microbiomejournal.biomedcentral.com/articles/10.1186/2049-2618-2-11)

3)     Greengenes: (http://greengenes.lbl.gov/cgi-bin/nph-index.cgi)

Please check the above websites for details of these tools/databases.
