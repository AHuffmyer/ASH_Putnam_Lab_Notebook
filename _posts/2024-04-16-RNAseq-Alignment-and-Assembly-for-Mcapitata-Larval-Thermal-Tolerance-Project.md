---
layout: post
title: Alignmnet and assembly of RNAseq reads for Montipora capitata larval thermal tolerance 2023 project 
date: '2024-04-16'
categories: Larval_Symbiont_TPC_2023
tags: Bioinformatics Mcapitata Molecular GeneExpression
---

This post details alignment and assembly steps of the bioinformatic pipeline for the *Montipora capitata* 2023 larval thermal tolerance project RNAseq. See my [notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#larval-symbiont-tpc-2023) and my [GitHub repo](https://github.com/AHuffmyer/larval_symbiont_TPC) for information on this project.

You can find QC for these files in [this post for original data delivery](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/RNAseq-QC-for-Mcapitata-Larval-Thermal-Tolerance-Project/) and [this post for second data delivery for additional top off sequencing](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Toppoff-Sequencing-RNAseq-QC-for-Mcapitata-Larval-Thermal-Tolerance-Project/).  

Files for this project include RNAseq files for 54 samples. Of these, 13 samples required additional sequencing to meet project deliverables for read depth. These files are denoted with an "s" after the sample prefix in the file name. 

Samples with two rounds of sequencing have been QC'd individually and all files pass QC checks. The files for each sample will be combined/concatenated prior to alignment and assembly.   

# 1. Concatenate files for samples with multiple files 

First, sym link all second sequencing ("s") trimmed files into the same directory as our original trimmed files.    

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/
mv trimmed-sequences-second-seq trimmed-sequences

cd trimmed-sequences/

ln -s /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/trimmed-sequences-second-seq/*fastq.gz /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences
```

Next, generate a file that has a list of the number of reads in each file. We will use this to verify that the concatenate functions work correctly.  

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/

interactive 

for file in $(ls *.fastq.gz); do echo $file && echo $(($(zcat $file | wc -l) / 4)); done > fastq_reads.txt
```

The output is as follows.   

| File                     | Trimmed Reads    |
|----------------------------|----------|
| `trim.R100_R1_001.fastq.gz`  | 22,620,372 |
| `trim.R100_R2_001.fastq.gz`  | 22,620,372 |
| `trim.R101_R1_001.fastq.gz`  | 24,721,395 |
| `trim.R101_R2_001.fastq.gz`  | 24,721,395 |
| `trim.R102_R1_001.fastq.gz`  | 21,719,378 |
| `trim.R102_R2_001.fastq.gz`  | 21,719,378 |
| `trim.R103_R1_001.fastq.gz`  | 15,045,349 |
| `trim.R103_R2_001.fastq.gz`  | 15,045,349 |
| `trim.R104_R1_001.fastq.gz`  | 20,320,917 |
| `trim.R104_R2_001.fastq.gz`  | 20,320,917 |
| `trim.R105_R1_001.fastq.gz`  | 19,295,017 |
| `trim.R105_R2_001.fastq.gz`  | 19,295,017 |
| `trim.R106_R1_001.fastq.gz`  | 23,294,462 |
| `trim.R106_R2_001.fastq.gz`  | 23,294,462 |
| `trim.R107_R1_001.fastq.gz`  | 4,822,107  |
| `trim.R107_R2_001.fastq.gz`  | 4,822,107  |
| `trim.R107s_R1_001.fastq.gz` | 11,457,851 |
| `trim.R107s_R2_001.fastq.gz` | 11,457,851 |
| `trim.R108_R1_001.fastq.gz`  | 19,644,360 |
| `trim.R108_R2_001.fastq.gz`  | 19,644,360 |
| `trim.R55_R1_001.fastq.gz`   | 10,429,847 |
| `trim.R55_R2_001.fastq.gz`   | 10,429,847 |
| `trim.R55s_R1_001.fastq.gz`  | 1,920,344  |
| `trim.R55s_R2_001.fastq.gz`  | 1,920,344  |
| `trim.R56_R1_001.fastq.gz`   | 10,950,200 |
| `trim.R56_R2_001.fastq.gz`   | 10,950,200 |
| `trim.R56s_R1_001.fastq.gz`  | 1,081,171  |
| `trim.R56s_R2_001.fastq.gz`  | 1,081,171  |
| `trim.R57_R1_001.fastq.gz`   | 9,818,404  |
| `trim.R57_R2_001.fastq.gz`   | 9,818,404  |
| `trim.R57s_R1_001.fastq.gz`  | 2,218,538  |
| `trim.R57s_R2_001.fastq.gz`  | 2,218,538  |
| `trim.R58_R1_001.fastq.gz`   | 11,599,234 |
| `trim.R58_R2_001.fastq.gz`   | 11,599,234 |
| `trim.R58s_R1_001.fastq.gz`  | 922,553   |
| `trim.R58s_R2_001.fastq.gz`  | 922,553   |
| `trim.R59_R1_001.fastq.gz`   | 4,181,880  |
| `trim.R59_R2_001.fastq.gz`   | 4,181,880  |
| `trim.R59s_R1_001.fastq.gz`  | 22,099,783 |
| `trim.R59s_R2_001.fastq.gz`  | 22,099,783 |
| `trim.R60_R1_001.fastq.gz`   | 10,371,860 |
| `trim.R60_R2_001.fastq.gz`   | 10,371,860 |
| `trim.R60s_R1_001.fastq.gz`  | 1,683,270  |
| `trim.R60s_R2_001.fastq.gz`  | 1,683,270  |
| `trim.R61_R1_001.fastq.gz`   | 12,840,605 |
| `trim.R61_R2_001.fastq.gz`   | 12,840,605 |
| `trim.R62_R1_001.fastq.gz`   | 10,586,054 |
| `trim.R62_R2_001.fastq.gz`   | 10,586,054 |
| `trim.R62s_R1_001.fastq.gz`  | 1,533,496  |
| `trim.R62s_R2_001.fastq.gz`  | 1,533,496  |
| `trim.R63_R1_001.fastq.gz`   | 16,366,473 |
| `trim.R63_R2_001.fastq.gz`   | 16,366,473 |
| `trim.R64_R1_001.fastq.gz`   | 21,582,713 |
| `trim.R64_R2_001.fastq.gz`   | 21,582,713 |
| `trim.R65_R1_001.fastq.gz`   | 20,881,937 |
| `trim.R65_R2_001.fastq.gz`   | 20,881,937 |
| `trim.R66_R1_001.fastq.gz`   | 21,685,720 |
| `trim.R66_R2_001.fastq.gz`   | 21,685,720 |
| `trim.R67_R1_001.fastq.gz`   | 5,427,517  |
| `trim.R67_R2_001.fastq.gz`   | 5,427,517  |
| `trim.R67s_R1_001.fastq.gz`  | 11,256,254 |
| `trim.R67s_R2_001.fastq.gz`  | 11,256,254 |
| `trim.R68_R1_001.fastq.gz`   | 20,670,181 |
| `trim.R68_R2_001.fastq.gz`   | 20,670,181 |
| `trim.R69_R1_001.fastq.gz`   | 26,128,049 |
| `trim.R69_R2_001.fastq.gz`   | 26,128,049 |
| `trim.R70_R1_001.fastq.gz`   | 22,666,926 |
| `trim.R70_R2_001.fastq.gz`   | 22,666,926 |
| `trim.R71_R1_001.fastq.gz`   | 19,026,441 |
| `trim.R71_R2_001.fastq.gz`   | 19,026,441 |
| `trim.R72_R1_001.fastq.gz`   | 22,047,415 |
| `trim.R72_R2_001.fastq.gz`   | 22,047,415 |
| `trim.R73_R1_001.fastq.gz`   | 23,215,360 |
| `trim.R73_R2_001.fastq.gz`   | 23,215,360 |
| `trim.R74_R1_001.fastq.gz`   | 23,372,268 |
| `trim.R74_R2_001.fastq.gz`   | 23,372,268 |
| `trim.R75_R1_001.fastq.gz`   | 7,020,548  |
| `trim.R75_R2_001.fastq.gz`   | 7,020,548  |
| `trim.R75s_R1_001.fastq.gz`  | 7,259,253  |
| `trim.R75s_R2_001.fastq.gz`  | 7,259,253  |
| `trim.R76_R1_001.fastq.gz`   | 22,091,635 |
| `trim.R76_R2_001.fastq.gz`   | 22,091,635 |
| `trim.R77_R1_001.fastq.gz`   | 26,679,183 |
| `trim.R77_R2_001.fastq.gz`   | 26,679,183 |
| `trim.R78_R1_001.fastq.gz`   | 23,235,087 |
| `trim.R78_R2_001.fastq.gz`   | 23,235,087 |
| `trim.R79_R1_001.fastq.gz`   | 16,737,093 |
| `trim.R79_R2_001.fastq.gz`   | 16,737,093 |
| `trim.R80_R1_001.fastq.gz`   | 23,504,936 |
| `trim.R80_R2_001.fastq.gz`   | 23,504,936 |
| `trim.R81_R1_001.fastq.gz`   | 21,501,629 |
| `trim.R81_R2_001.fastq.gz`   | 21,501,629 |
| `trim.R82_R1_001.fastq.gz`   | 23,350,580 |
| `trim.R82_R2_001.fastq.gz`   | 23,350,580 |
| `trim.R83_R1_001.fastq.gz`   | 5,263,676  |
| `trim.R83_R2_001.fastq.gz`   | 5,263,676  |
| `trim.R83s_R1_001.fastq.gz`  | 10,665,955 |
| `trim.R83s_R2_001.fastq.gz` | 10,665,955 |
| `trim.R84_R1_001.fastq.gz`   | 21,296,217 |
| `trim.R84_R2_001.fastq.gz`   | 21,296,217 |
| `trim.R85_R1_001.fastq.gz`   | 23,839,002 |
| `trim.R85_R2_001.fastq.gz`   | 23,839,002 |
| `trim.R86_R1_001.fastq.gz`   | 21,015,291 |
| `trim.R86_R2_001.fastq.gz`   | 21,015,291 |
| `trim.R87_R1_001.fastq.gz`   | 16,971,668 |
| `trim.R87_R2_001.fastq.gz`   | 16,971,668 |
| `trim.R88_R1_001.fastq.gz`   | 19,964,497 |
| `trim.R88_R2_001.fastq.gz`   | 19,964,497 |
| `trim.R89_R1_001.fastq.gz`   | 21,793,515 |
| `trim.R89_R2_001.fastq.gz`   | 21,793,515 |
| `trim.R90_R1_001.fastq.gz`   | 21,601,078 |
| `trim.R90_R2_001.fastq.gz`   | 21,601,078 |
| `trim.R91_R1_001.fastq.gz`   | 4,829,073  |
| `trim.R91_R2_001.fastq.gz`   | 4,829,073  |
| `trim.R91s_R1_001.fastq.gz`  | 11,581,372 |
| `trim.R91s_R2_001.fastq.gz`  | 11,581,372 |
| `trim.R92_R1_001.fastq.gz`   | 18,998,179 |
| `trim.R92_R2_001.fastq.gz`   | 18,998,179 |
| `trim.R93_R1_001.fastq.gz`   | 21,278,345 |
| `trim.R93_R2_001.fastq.gz`   | 21,278,345 |
| `trim.R94_R1_001.fastq.gz`   | 21,321,504 |
| `trim.R94_R2_001.fastq.gz`   | 21,321,504 |
| `trim.R95_R1_001.fastq.gz`   | 18,342,977 |
| `trim.R95_R2_001.fastq.gz`   | 18,342,977 |
| `trim.R96_R1_001.fastq.gz`   | 23,025,861 |
| `trim.R96_R2_001.fastq.gz`   | 23,025,861 |
| `trim.R97_R1_001.fastq.gz`   | 21,450,165 |
| `trim.R97_R2_001.fastq.gz`   | 21,450,165 |
| `trim.R98_R1_001.fastq.gz`   | 23,491,288 |
| `trim.R98_R2_001.fastq.gz`   | 23,491,288 |
| `trim.R99_R1_001.fastq.gz`   | 5,743,661  |
| `trim.R99_R2_001.fastq.gz`   | 5,743,661  |
| `trim.R99s_R1_001.fastq.gz`  | 9,230,551  |
| `trim.R99s_R2_001.fastq.gz`  | 9,230,551  |

The samples with "s" indicate second data delivery. Notice that samples with lowest read depth in the original data delivery have much greater read depth in the second round of sequencing to meet project deliverables at Azenta.  

Now, concatenate the files for each sample by concatenating R1 files together and R2 files together for samples that had additional sequencing. Concatenate files manually since we have a small number and I want to check each one. Name the concatenated files with "c" after the sample prefix.  

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences

interactive 

cat trim.R55*R1*.fastq.gz >> trim.R55c_R1_001.fastq.gz #done
cat trim.R55*R2*.fastq.gz >> trim.R55c_R2_001.fastq.gz #done

cat trim.R56*R1*.fastq.gz >> trim.R56c_R1_001.fastq.gz #done
cat trim.R56*R2*.fastq.gz >> trim.R56c_R2_001.fastq.gz #done

cat trim.R57*R1*.fastq.gz >> trim.R57c_R1_001.fastq.gz #done
cat trim.R57*R2*.fastq.gz >> trim.R57c_R2_001.fastq.gz #done

cat trim.R58*R1*.fastq.gz >> trim.R58c_R1_001.fastq.gz #done
cat trim.R58*R2*.fastq.gz >> trim.R58c_R2_001.fastq.gz #done

cat trim.R59*R1*.fastq.gz >> trim.R59c_R1_001.fastq.gz #done
cat trim.R59*R2*.fastq.gz >> trim.R59c_R2_001.fastq.gz #done

cat trim.R60*R1*.fastq.gz >> trim.R60c_R1_001.fastq.gz #done
cat trim.R60*R2*.fastq.gz >> trim.R60c_R2_001.fastq.gz #done

cat trim.R62*R1*.fastq.gz >> trim.R62c_R1_001.fastq.gz #done
cat trim.R62*R2*.fastq.gz >> trim.R62c_R2_001.fastq.gz #done

cat trim.R67*R1*.fastq.gz >> trim.R67c_R1_001.fastq.gz #done
cat trim.R67*R2*.fastq.gz >> trim.R67c_R2_001.fastq.gz #done

cat trim.R75*R1*.fastq.gz >> trim.R75c_R1_001.fastq.gz #done
cat trim.R75*R2*.fastq.gz >> trim.R75c_R2_001.fastq.gz #done

cat trim.R83*R1*.fastq.gz >> trim.R83c_R1_001.fastq.gz #done
cat trim.R83*R2*.fastq.gz >> trim.R83c_R2_001.fastq.gz #done

cat trim.R91*R1*.fastq.gz >> trim.R91c_R1_001.fastq.gz #done
cat trim.R91*R2*.fastq.gz >> trim.R91c_R2_001.fastq.gz #done

cat trim.R99*R1*.fastq.gz >> trim.R99c_R1_001.fastq.gz #done
cat trim.R99*R2*.fastq.gz >> trim.R99c_R2_001.fastq.gz #done

cat trim.R107*R1*.fastq.gz >> trim.R107c_R1_001.fastq.gz #done
cat trim.R107*R2*.fastq.gz >> trim.R107c_R2_001.fastq.gz #done

exit

```

The notation for these is to concatenate the first and second data delivery together for R1 and then for R2.  

`ls trim.R55*R1*.fastq.gz` generates `trim.R55_R1_001.fastq.gz  trim.R55s_R1_001.fastq.gz` 

and 

`ls trim.R55*R2*.fastq.gz` generates `trim.R55_R2_001.fastq.gz  trim.R55s_R2_001.fastq.gz`

Test that the number of reads increases for one file. 

```
cat trim.R55*R1*.fastq.gz >> trim.R55c_R1_001.fastq.gz

$(($(zcat trim.R55c_R1_001.fastq.gz  | wc -l) / 4)) 

```

R55 R1 original was 10,429,847 reads and R55 R1 second delivery was 1,920,344 reads. It is now 12,350,191 reads. 12,350,191 reads is expected. This code worked, proceed with all files.    

Now, move any files from the concatenation out of this directory. 

For files that were concatenated, view the number of reads against the expected value (original + second).  

```
interactive 
# view concatenated reads 

for file in $(ls *c*.fastq.gz); do echo $file && echo $(($(zcat $file | wc -l) / 4)); done > fastq_cat_reads.txt

exit
```

All concatenated files have the expected number of reads.  

| Sample | Expected Total  | Cancatenated Total |
|--------|-----------------|---------------------------------|
| R55    | 12,350,191        | 12,350,191                        |
| R56    | 12,031,371        | 12,031,371                        |
| R57    | 12,036,942        | 12,036,942                        |
| R58    | 12,521,787        | 12,521,787                        |
| R59    | 26,281,663        | 26,281,663                        |
| R60    | 12,055,130        | 12,055,130                        |
| R62    | 12,119,550        | 12,119,550                        |
| R67    | 16,683,771        | 16,683,771                        |
| R75    | 14,279,801        | 14,279,801                        |
| R83    | 15,929,631        | 15,929,631                        |
| R91    | 16,410,445        | 16,410,445                        |
| R99    | 14,974,,212        | 14,974,212                        |
| R107   | 16,279,958        | 16,279,958                        |

Finally, make a new directory with sym links to the files that will go forward for mapping and alignment (concatenated files for those with multiple data deliveries).   

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences

mkdir cat-sequences

cd cat-sequences

ln -s /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/*fastq.gz /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/cat-sequences

```

Remove files that will not be used.  

```
#remove the second data delivery files 
rm *s*.fastq.gz

#manually remove the first data delivery files 
rm trim.R55_R*fastq.gz
rm trim.R56_R*fastq.gz
rm trim.R57_R*fastq.gz
rm trim.R58_R*fastq.gz
rm trim.R59_R*fastq.gz
rm trim.R60_R*fastq.gz
rm trim.R62_R*fastq.gz
rm trim.R67_R*fastq.gz
rm trim.R75_R*fastq.gz
rm trim.R83_R*fastq.gz
rm trim.R91_R*fastq.gz
rm trim.R99_R*fastq.gz
rm trim.R107_R*fastq.gz
```

The files we want to proceed with are now in `/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/cat-sequences` and are as follows:  

```
trim.R100_R1_001.fastq.gz   trim.R57c_R1_001.fastq.gz  trim.R68_R1_001.fastq.gz   trim.R79_R1_001.fastq.gz   trim.R90_R1_001.fastq.gz
trim.R100_R2_001.fastq.gz   trim.R57c_R2_001.fastq.gz  trim.R68_R2_001.fastq.gz   trim.R79_R2_001.fastq.gz   trim.R90_R2_001.fastq.gz
trim.R101_R1_001.fastq.gz   trim.R58c_R1_001.fastq.gz  trim.R69_R1_001.fastq.gz   trim.R80_R1_001.fastq.gz   trim.R91c_R1_001.fastq.gz
trim.R101_R2_001.fastq.gz   trim.R58c_R2_001.fastq.gz  trim.R69_R2_001.fastq.gz   trim.R80_R2_001.fastq.gz   trim.R91c_R2_001.fastq.gz
trim.R102_R1_001.fastq.gz   trim.R59c_R1_001.fastq.gz  trim.R70_R1_001.fastq.gz   trim.R81_R1_001.fastq.gz   trim.R92_R1_001.fastq.gz
trim.R102_R2_001.fastq.gz   trim.R59c_R2_001.fastq.gz  trim.R70_R2_001.fastq.gz   trim.R81_R2_001.fastq.gz   trim.R92_R2_001.fastq.gz
trim.R103_R1_001.fastq.gz   trim.R60c_R1_001.fastq.gz  trim.R71_R1_001.fastq.gz   trim.R82_R1_001.fastq.gz   trim.R93_R1_001.fastq.gz
trim.R103_R2_001.fastq.gz   trim.R60c_R2_001.fastq.gz  trim.R71_R2_001.fastq.gz   trim.R82_R2_001.fastq.gz   trim.R93_R2_001.fastq.gz
trim.R104_R1_001.fastq.gz   trim.R61_R1_001.fastq.gz   trim.R72_R1_001.fastq.gz   trim.R83c_R1_001.fastq.gz  trim.R94_R1_001.fastq.gz
trim.R104_R2_001.fastq.gz   trim.R61_R2_001.fastq.gz   trim.R72_R2_001.fastq.gz   trim.R83c_R2_001.fastq.gz  trim.R94_R2_001.fastq.gz
trim.R105_R1_001.fastq.gz   trim.R62c_R1_001.fastq.gz  trim.R73_R1_001.fastq.gz   trim.R84_R1_001.fastq.gz   trim.R95_R1_001.fastq.gz
trim.R105_R2_001.fastq.gz   trim.R62c_R2_001.fastq.gz  trim.R73_R2_001.fastq.gz   trim.R84_R2_001.fastq.gz   trim.R95_R2_001.fastq.gz
trim.R106_R1_001.fastq.gz   trim.R63_R1_001.fastq.gz   trim.R74_R1_001.fastq.gz   trim.R85_R1_001.fastq.gz   trim.R96_R1_001.fastq.gz
trim.R106_R2_001.fastq.gz   trim.R63_R2_001.fastq.gz   trim.R74_R2_001.fastq.gz   trim.R85_R2_001.fastq.gz   trim.R96_R2_001.fastq.gz
trim.R107c_R1_001.fastq.gz  trim.R64_R1_001.fastq.gz   trim.R75c_R1_001.fastq.gz  trim.R86_R1_001.fastq.gz   trim.R97_R1_001.fastq.gz
trim.R107c_R2_001.fastq.gz  trim.R64_R2_001.fastq.gz   trim.R75c_R2_001.fastq.gz  trim.R86_R2_001.fastq.gz   trim.R97_R2_001.fastq.gz
trim.R108_R1_001.fastq.gz   trim.R65_R1_001.fastq.gz   trim.R76_R1_001.fastq.gz   trim.R87_R1_001.fastq.gz   trim.R98_R1_001.fastq.gz
trim.R108_R2_001.fastq.gz   trim.R65_R2_001.fastq.gz   trim.R76_R2_001.fastq.gz   trim.R87_R2_001.fastq.gz   trim.R98_R2_001.fastq.gz
trim.R55c_R1_001.fastq.gz   trim.R66_R1_001.fastq.gz   trim.R77_R1_001.fastq.gz   trim.R88_R1_001.fastq.gz   trim.R99c_R1_001.fastq.gz
trim.R55c_R2_001.fastq.gz   trim.R66_R2_001.fastq.gz   trim.R77_R2_001.fastq.gz   trim.R88_R2_001.fastq.gz   trim.R99c_R2_001.fastq.gz
trim.R56c_R1_001.fastq.gz   trim.R67c_R1_001.fastq.gz  trim.R78_R1_001.fastq.gz   trim.R89_R1_001.fastq.gz
trim.R56c_R2_001.fastq.gz   trim.R67c_R2_001.fastq.gz  trim.R78_R2_001.fastq.gz   trim.R89_R2_001.fastq.gz
```

# 2. Align reads to M. capitata genome using hisat2

### Download reference genome and functional annotations

I first downloaded the *Montipora capitata* reference genome details in [Stephens et al. 2022](https://academic.oup.com/gigascience/article/doi/10.1093/gigascience/giac098/6815755) and available as Version 3 [online here](http://cyanophora.rutgers.edu/montipora/).  

The URL's of the files we need to download are:  

- Genome assembly: `http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.assembly.fasta.gz` 

- GFF: `http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.genes.gff3.gz`  

Other files available include the following. We will use the functional annotation files in downstream steps.    

- Functional annotation: `http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.genes.EggNog_results.txt.gz` 

- Protein: `http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.genes.pep.faa.gz`

- Nucleotide CDS: `http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.genes.cds.fna.gz`  

- CD Search Functional Annotation: `http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.genes.Conserved_Domain_Search_results.txt.gz`

- KAAS Functional Annotation: `http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.genes.KEGG_results.txt.gz` 

Download files to Andromeda folder.  

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/
mkdir references

cd references

wget http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.assembly.fasta.gz

wget http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.genes.gff3.gz

gunzip Montipora_capitata_HIv3.assembly.fasta.gz
gunzip Montipora_capitata_HIv3.genes.gff3.gz

``` 

These files are now available at `/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references` as `Montipora_capitata_HIv3.assembly.fasta` and `Montipora_capitata_HIv3.genes.gff3`.  

### Alignment with hisat2

Write a script for alignment. These are based on [Jill Ashey's scripts](https://github.com/JillAshey/JillAshey_Putnam_Lab_Notebook/blob/master/_posts/2024-01-18-RNASeq-Pacuta-Hawaii-2022.md). 

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/scripts

nano align.sh
```

The manual for [hisat2 can be found here](https://daehwankimlab.github.io/hisat2/manual/). Here, we will align trimmed and concatenated sequences generated above. We will use multiple processors (`-p 8`) and `--rna-strandness RF` to specify strand-specific information.  

```
#!/bin/bash
#SBATCH -t 120:00:00
#SBATCH --nodes=1 --ntasks-per-node=10
#SBATCH --export=NONE
#SBATCH --mem=100GB
#SBATCH --mail-type=BEGIN,END,FAIL #email you when job starts, stops and/or fails
#SBATCH --mail-user=ashuffmyer@uri.edu #your email to send notifications
#SBATCH --account=putnamlab
#SBATCH -D /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/cat-sequences/       
#SBATCH -o align-out.out
#SBATCH -e align-error.error

# load modules needed
module load HISAT2/2.2.1-foss-2019b #Alignment to reference genome: HISAT2
module load SAMtools/1.9-foss-2018b #Preparation of alignment for assembly: SAMtools

echo "Building genome reference" $(date)

# index the reference genome for Pacuta output index to working directory
hisat2-build -f /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Montipora_capitata_HIv3.assembly.fasta /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Mcapitata_ref
echo "Referece genome indexed. Starting alingment" $(date)

# This script exports alignments as bam files
# sorts the bam file because Stringtie takes a sorted file for input (--dta)
# removes the sam file because it is no longer needed

array=($(ls /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/cat-sequences/*fastq.gz)) # call the clean sequences - make an array to align

for i in ${array[@]}; do
        sample_name=`echo $i| awk -F [.] '{print $2}'`
        hisat2 -p 8 --rna-strandness RF --dta -x /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Mcapitata_ref -1 ${i} -2 $(echo ${i}|sed s/_R1/_R2/) -S ${sample_name}.sam
        samtools sort -@ 8 -o ${sample_name}.bam ${sample_name}.sam
                echo "${i} bam-ified!"
        rm ${sample_name}.sam
done

echo "Alignment complete!" $(date)
```

```
sbatch align.sh
```

Job ID 312427 started at 15:45 on 17 April 2024.  

View mapping percentages after job was completed. 

```
interactive 

cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/cat-sequences/

module load SAMtools/1.9-foss-2018b 
#Preparation of alignment for assembly: SAMtools

for i in *.bam; do
    echo "${i}" >> mapped_reads_counts
    samtools flagstat ${i} | grep "mapped (" >> mapped_reads_counts
done

```

View the results. 

RESULTS HERE 