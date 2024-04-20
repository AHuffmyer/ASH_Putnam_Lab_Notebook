---
layout: post
title: Alignment and assembly of RNAseq reads for Montipora capitata larval thermal tolerance 2023 project 
date: '2024-04-20'
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

### Fix GFF3 format  

*Note that I have to first correct the format of the .gff3 file.* I had to do this [before in a previous analysis](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Analyzing-TagSeq-data-with-new-annotation/) because it generated many unknown gene names even though the sequences mapped to the reference. We have to correct the format of column 9.   

Download gff to local computer a the website listed above to `~/MyProjects/larval_symbiont_TPC/scripts/bioinformatics`.  

```
cd ~/MyProjects/larval_symbiont_TPC/data/rna_seq

gunzip Montipora_capitata_HIv3.genes.gff3.gz

```

I next ran [this R script](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/bioinformatics/fix_gff_format.Rmd) in the GitHub R project for this data.  

```
#This script add transcript and gene id into GFF file for alignment.  #Here, I'll be adding transcript_id= and gene_id= to 'gene' column because we needs that label to map our RNAseq data  #Load libraries and data. library(tidyverse)library(R.utils)#Load  gene gff filegff <- read.csv(file="data/rna_seq/Montipora_capitata_HIv3.genes.gff3", header=FALSE, sep="\t") #Rename columns colnames(gff) <- c("scaffold", "Gene.Predict", "id", "gene.start","gene.stop", "pos1", "pos2","pos3", "gene")head(gff)#Create transcript ID  gff$transcript_id <- sub(";.*", "", gff$gene)gff$transcript_id <- gsub("ID=", "", gff$transcript_id) #remove ID= gff$transcript_id <- gsub("Parent=", "", gff$transcript_id) #remove ID= head(gff)#Create Parent ID gff$parent_id <- sub(".*Parent=", "", gff$gene)gff$parent_id <- sub(";.*", "", gff$parent_id)gff$parent_id <- gsub("ID=", "", gff$parent_id) #remove ID= head(gff)#Now add these values into the gene column separated by semicolons.  gff <- gff %>%   mutate(gene = ifelse(id != "gene", paste0(gene, ";transcript_id=", gff$transcript_id, ";gene_id=", gff$parent_id),  paste0(gene)))head(gff)#Now remove the transcript and parent ID separate columns.  gff<-gff %>%  select(!transcript_id)%>%  select(!parent_id)head(gff)#Save file. Then upload this to Andromeda for use in bioinformatic steps.  write.table(gff, file="data/rna_seq/Montipora_capitata_HIv3.genes_fixed.gff3", sep="\t", col.names = FALSE, row.names=FALSE, quote=FALSE)```

Upload file to Andromeda. 

```
scp ~/MyProjects/larval_symbiont_TPC/data/rna_seq/Montipora_capitata_HIv3.genes_fixed.gff3 ashuffmyer@ssh3.hac.uri.edu:/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/ 
```  

### Transfer files to Andromeda  

Download reference genome files to Andromeda folder.  

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/
mkdir references

cd references

wget http://cyanophora.rutgers.edu/montipora/Montipora_capitata_HIv3.assembly.fasta.gz

gunzip Montipora_capitata_HIv3.assembly.fasta.gz

``` 

These files are now available at `/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references` as `Montipora_capitata_HIv3.assembly.fasta` and `Montipora_capitata_HIv3.genes_fixed.gff3`.  

### Alignment with hisat2

Write a script for alignment. These are based on [Jill Ashey's scripts](https://github.com/JillAshey/JillAshey_Putnam_Lab_Notebook/blob/master/_posts/2024-01-18-RNASeq-Pacuta-Hawaii-2022.md). 

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/scripts

nano align.sh
```

The manual for [hisat2 can be found here](https://daehwankimlab.github.io/hisat2/manual/). Here, we will align trimmed and concatenated sequences generated above. We will use multiple processors (`-p 8`) and `--rna-strandness RF` to specify strand-specific information. We will keep the .sam file. The options used are below:  

-  `-f` Reads are FASTA files - used for building reference in `hisat2-build`
- `x` The basename of the index for the reference genome 
- `-p 8` Use multiple processors (8) 
- `--rna-strandness RF` Specify strand-specific information: the default is unstranded. For paired-end reads, use either FR or RF. With this option being used, every read alignment will have an XS attribute tag: ’+’ means a read belongs to a transcript on ‘+’ strand of genome. ‘-‘ means a read belongs to a transcript on ‘-‘ strand of genome.
- `--dta` Downstream transcriptome assembly. Report alignments tailored for transcript assemblers including StringTie. With this option, HISAT2 requires longer anchor lengths for de novo discovery of splice sites. This leads to fewer alignments with short-anchors, which helps transcript assemblers improve significantly in computation and memory usage. We are using StringTie below.  
- `-1` Comma-separated list of files containing mate 1s (filename usually includes _1) (R1 files) 
- `-2` Comma-separated list of files containing mate 2s (filename usually includes _2) (R2 files) 
- `sed s/_R1/_R2/`: This is a sed command that performs a search and replace operation. It replaces `_R1` with `_R2`. This allows the R1 file to be specified in the input array and the program will read in the corresponding R2 file. 
- `-S` File to write SAM alignments to.
- In the `samtools sort` function, `-@` sets number of threads for parallel processing and `-o` sets the output file.   


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

#echo "Building genome reference" $(date)

# index the reference genome for Pacuta output index to working directory
hisat2-build -f /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Montipora_capitata_HIv3.assembly.fasta /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Mcapitata_ref

echo "Referece genome indexed. Starting alingment" $(date)

# This script exports alignments as bam files
# sorts the bam file because Stringtie takes a sorted file for input (--dta)
# removes the sam file because it is no longer needed

array=($(ls /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/cat-sequences/*_R1_001.fastq.gz)) # call the clean sequences - make an array to align

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

Job ID 312488 started at 11:00 on 19 April 2024.  

Job finished at 03:00 on 20 April 2024.  

Obtain mapping percentages after job was completed. Use the `samtools flagstat` function that does a full pass through the input file to calculate and print statistics to stdout. Provides counts for each of 13 categories based primarily on bit flags in the FLAG field. Each category in the output is broken down into QC pass and QC fail, which is presented as "#PASS + #FAIL" followed by a description of the category.

```
interactive 

cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/cat-sequences/

module load SAMtools/1.9-foss-2018b 

for i in *.bam; do
    echo "${i}" >> mapped_reads_counts
    samtools flagstat ${i} | grep "mapped (" >> mapped_reads_counts
done

```

View the results. 

```
less mapped_reads_counts

R100_R1_001.bam
48546456 + 0 mapped (88.52% : N/A)
R101_R1_001.bam
50823099 + 0 mapped (88.61% : N/A)
R102_R1_001.bam
47629227 + 0 mapped (88.93% : N/A)
R103_R1_001.bam
34061219 + 0 mapped (88.57% : N/A)
R104_R1_001.bam
38331360 + 0 mapped (83.35% : N/A)
R105_R1_001.bam
38291367 + 0 mapped (86.94% : N/A)
R106_R1_001.bam
44981515 + 0 mapped (85.40% : N/A)
R107c_R1_001.bam
34154018 + 0 mapped (88.21% : N/A)
R108_R1_001.bam
39118124 + 0 mapped (86.82% : N/A)
R55c_R1_001.bam
24191832 + 0 mapped (85.93% : N/A)
R56c_R1_001.bam
21720710 + 0 mapped (80.27% : N/A)
R57c_R1_001.bam
22995053 + 0 mapped (84.52% : N/A)
R58c_R1_001.bam
24584697 + 0 mapped (85.47% : N/A)
R59c_R1_001.bam
50409476 + 0 mapped (84.59% : N/A)
R60c_R1_001.bam
23258702 + 0 mapped (84.56% : N/A)
R61_R1_001.bam
28733929 + 0 mapped (89.69% : N/A)
R62c_R1_001.bam
25793167 + 0 mapped (87.32% : N/A)
R63_R1_001.bam
31334562 + 0 mapped (84.81% : N/A)
R64_R1_001.bam
42529670 + 0 mapped (86.47% : N/A)
R65_R1_001.bam
41257485 + 0 mapped (86.56% : N/A)
R66_R1_001.bam
43227744 + 0 mapped (87.96% : N/A)
R67c_R1_001.bam
32687009 + 0 mapped (87.56% : N/A)
R68_R1_001.bam
40790317 + 0 mapped (86.25% : N/A)
R69_R1_001.bam
56513901 + 0 mapped (87.15% : N/A)
R70_R1_001.bam
44931912 + 0 mapped (87.46% : N/A)
R71_R1_001.bam
37858692 + 0 mapped (88.31% : N/A)
R72_R1_001.bam
44050034 + 0 mapped (87.44% : N/A)
R73_R1_001.bam
46100943 + 0 mapped (85.56% : N/A)
R74_R1_001.bam
45238135 + 0 mapped (83.95% : N/A)
R75c_R1_001.bam
27623064 + 0 mapped (84.72% : N/A)
R76_R1_001.bam
42983597 + 0 mapped (84.87% : N/A)
R77_R1_001.bam
50734492 + 0 mapped (83.71% : N/A)
R78_R1_001.bam
45822615 + 0 mapped (86.63% : N/A)
R79_R1_001.bam
37810491 + 0 mapped (87.37% : N/A)
R80_R1_001.bam
47352909 + 0 mapped (87.42% : N/A)
R81_R1_001.bam
41085082 + 0 mapped (83.82% : N/A)
R82_R1_001.bam
46121413 + 0 mapped (86.04% : N/A)
R83c_R1_001.bam
32597478 + 0 mapped (87.63% : N/A)
R84_R1_001.bam
43999246 + 0 mapped (87.44% : N/A)
R85_R1_001.bam
47890197 + 0 mapped (87.61% : N/A)
R86_R1_001.bam
41182836 + 0 mapped (87.11% : N/A)
R87_R1_001.bam
33690883 + 0 mapped (87.49% : N/A)
R88_R1_001.bam
46415439 + 0 mapped (88.28% : N/A)
R89_R1_001.bam
43717178 + 0 mapped (87.52% : N/A)
R90_R1_001.bam
42916789 + 0 mapped (87.57% : N/A)
R91c_R1_001.bam
34549408 + 0 mapped (88.12% : N/A)
R92_R1_001.bam
41520832 + 0 mapped (88.47% : N/A)
R93_R1_001.bam
67123654 + 0 mapped (92.22% : N/A)
R94_R1_001.bam
43946968 + 0 mapped (86.17% : N/A)
R95_R1_001.bam
36354614 + 0 mapped (87.17% : N/A)
R96_R1_001.bam
52937351 + 0 mapped (87.08% : N/A)
R97_R1_001.bam
45017808 + 0 mapped (88.38% : N/A)
R98_R1_001.bam
45239251 + 0 mapped (84.92% : N/A)
R99c_R1_001.bam
35015923 + 0 mapped (90.36% : N/A)

```

All mapping percentages >80%! This are great for coral alignment and are the upper range of what I have seen for this species.  

Note that the output files are named with "R1" because that is the name of files in the input array. R1 and R2 reads were used in the mapping step. If you want, you could remove R1 from the names.  

# 3. Assemble and quantify read counts with stringtie 

First, sym link .bam files to a new bam-files directory. I am using sym links throughout this pipeline so that the original scripts will overwrite existing files if something needs to be re run. If desired, you can keep all files in the same directory. I like to keep things in discrete directories to keep it organized.   

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq

mkdir bam-files

cd bam-files

ln -s /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/trimmed-sequences/cat-sequences/*.bam /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/bam-files
```

Write a script for assembly. 

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/scripts

nano assembly.sh
```

We will use stringtie for assembly with the following options:  

- `-p 8` for using multiple processors 
- `-e` this option directs StringTie to operate in expression estimation mode; this limits the processing of read alignments to estimating the coverage of the transcripts given with the -G option (hence this option requires -G).
- `-B` This switch enables the output of Ballgown input table files (.ctab) containing coverage data for the reference transcripts given with the -G option. (See the Ballgown documentation for a description of these files.) With this option StringTie can be used as a direct replacement of the tablemaker program included with the Ballgown distribution. If the option -o is given as a full path to the output transcript file, StringTie will write the .ctab files in the same directory as the output GTF.
- `-G` Use a reference annotation file (in GTF or GFF3 format) to guide the assembly process. The output will include expressed reference transcripts as well as any novel transcripts that are assembled. This option is required by options -B, -b, -e, -C.
- `-A` Gene abundances will be reported (tab delimited format) in the output file with the given name.
- `-o` output file name for the merged transcripts GTF (default: stdout)

```
#!/bin/bash
#SBATCH -t 120:00:00
#SBATCH --nodes=1 --ntasks-per-node=10
#SBATCH --export=NONE
#SBATCH --mem=100GB
#SBATCH --mail-type=BEGIN,END,FAIL #email you when job starts, stops and/or fails
#SBATCH --mail-user=ashuffmyer@uri.edu #your email to send notifications
#SBATCH --account=putnamlab
#SBATCH -D /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/bam-files       
#SBATCH -o assemble-out.out
#SBATCH -e assemble-error.error

module load StringTie/2.2.1-GCC-11.2.0

echo "Assembling transcripts using stringtie" $(date)

cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/bam-files

array=($(ls *.bam)) #Make an array of sequences to assemble

for i in ${array[@]}; do 
        sample_name=`echo $i| awk -F [_] '{print $1}'`
	stringtie -p 8 -e -B -G /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Montipora_capitata_HIv3.genes_fixed.gff3 -A ${sample_name}.gene_abund.tab -o ${sample_name}.gtf ${i}
        echo "StringTie assembly for seq file ${i}" $(date)
done

echo "Assembly for each sample complete " $(date)
```

```
sbatch assembly.sh
```

Job ID 312546 started at 14:00 on 20 April 2024. Completed 16:00 on April 2024. 


Sym link .gtf files to new directory. 

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/
mkdir gtf-files
cd gtf-files

ln -s /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/bam-files/*.gtf /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/gtf-files

ln -s /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/bam-files/*.tab /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/gtf-files
```

# 4. Prepare .gtf files and generate gene count matrix 

Make list of .gtf files. I am doing these steps individually, but can be added to a bash script if desired.  

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/gtf-files

ls *.gtf > gtf_list.txt
```

Merge .gtf's. Use same stringtie settings as above and generate a merged gtf file.   

```
interactive 

module load StringTie/2.1.4-GCC-9.3.0

stringtie --merge -e -p 8 -G /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Montipora_capitata_HIv3.genes_fixed.gff3 -o Mcapitata_merged.gtf gtf_list.txt 

```

Assess the accuracy of the merged assembly with `gffcompare`.

```
interactive 

module purge
module load GffCompare/0.12.1-GCCcore-8.3.0

gffcompare -r /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Montipora_capitata_HIv3.genes_fixed.gff3 Mcapitata_merged.gtf 

  54384 reference transcripts loaded.
  2 duplicate reference transcripts discarded.
  54384 query transfrags loaded.
```

View the merged file.  

```
less gffcmp.stats 
```

```
# gffcompare v0.12.1 | Command line was:
#gffcompare -r /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/references/Montipora_capitata_HIv3.genes_fixed.gff3 Mcapitata_merged.gtf
#

#= Summary for dataset: Mcapitata_merged.gtf 
#     Query mRNAs :   54384 in   54185 loci  (36023 multi-exon transcripts)
#            (141 multi-transcript loci, ~1.0 transcripts per locus)
# Reference mRNAs :   54382 in   54185 loci  (36023 multi-exon)
# Super-loci w/ reference transcripts:    54185
#-----------------| Sensitivity | Precision  |
        Base level:   100.0     |   100.0    |
        Exon level:   100.0     |   100.0    |
      Intron level:   100.0     |   100.0    |
Intron chain level:   100.0     |   100.0    |
  Transcript level:   100.0     |   100.0    |
       Locus level:   100.0     |   100.0    |

     Matching intron chains:   36023
       Matching transcripts:   54358
              Matching loci:   54183

          Missed exons:       0/256029  (  0.0%)
           Novel exons:       0/256028  (  0.0%)
        Missed introns:       0/201643  (  0.0%)
         Novel introns:       0/201643  (  0.0%)
           Missed loci:       0/54185   (  0.0%)
            Novel loci:       0/54185   (  0.0%)

 Total union super-loci across all input datasets: 54185 
54384 out of 54384 consensus transcripts written in gffcmp.annotated.gtf (0 discarded as redundant)
```

Make gtf list text file for gene count matrix creation; still in interactive mode. 

```
for filename in R*.gtf; do echo $filename $PWD/$filename; done > listGTF.txt
```

Download the prepDE.py script from [here](https://github.com/gpertea/stringtie/blob/master/prepDE.py) and put it in the stringtie output folder. 

In a terminal not logged into Andromeda:  

```
scp ~/MyProjects/larval_symbiont_TPC/scripts/bioinformatics/prepDE.py ashuffmyer@ssh3.hac.uri.edu:/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/scripts/ 

```

Load python and compile the gene count matrix

```
interactive 

cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/gtf-files 

module purge
module load Python/2.7.18-GCCcore-9.3.0

python /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/scripts/prepDE.py -g Mcapitata2023_gene_count_matrix.csv -i listGTF.txt
```

We do not have any STRG gene names, which means that fixing the GFF file allowed for correct identification of annotated genes/transcripts.  

Transfer gene count matrix to desktop.  

```
scp ashuffmyer@ssh3.hac.uri.edu:/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/gtf-files/Mcapitata2023_gene_count_matrix.csv ~/MyProjects/larval_symbiont_TPC/data/rna_seq

```

