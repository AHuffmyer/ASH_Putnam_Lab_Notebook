---
layout: post
title: Geoduck RRBS Library Prep - part 3
date: '2016-12-15'
categories: Processing
tags: [DNA, DNA Methylation, geoduck, P. generosa, RRBS]
---

Prepping 40 RRBS libraries for geoduck juvenile OA acclimatization study.

20161215
100ng was used as the starting amount of DNA for each sample and the volume of water in each reaction was adjusted as necessary. 0.5% unmethylated lambda DNA was spiked in (w/w DNA) to determine conversion efficiency by estimating the error rate at which a C count occurs at an unmethylated C position.

# Step 1 MSPI DNA Cutting
MSPI restriction endonuclease in NEBuffer2

* MspI - 25,000U (NEB cat: R0106L)
* NEBuffer2 10x (NEB cat: B7002S)

## Reaction Mix
* Samples were mixed as follows to a total reaction volume of 30µl. Samples were incubated at 37°C starting at 18:00 
* Lambda DNA (581ng/µl) was diluted 1:1160 for a concentration of 0.5ng/µl and 1µl added to each sample
* Unmethylated lamda phage DNA (Promega cat: D1501)

	* 3µl NEBuffer2
	* 1µl MSPI 20U/µl
	* 16µl DNA (100ng)
	* 10µl of H20
	* 30µl total

Date|EPI.Tube.Num|Tank|Treatment|TimePoint|Homogenization|DNA.Extraction|DNA.QC|DNA.Conc.ng.µl|DNA.vol.µl|DNA.amount.µg|vol for 100ng|lambda.0.5%|Water.µl|NSBuffer2.µl|MSPI.µl
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---
16-Mar-16|EPI_41|Time0|Field|Day0|20160912|20161025|20161026|13.6|140|1.904|7.4|1|17.6|3|1
16-Mar-16|EPI_42|Time0|Field|Day0|20160912|20161028|20161028|79|140|11.06|1.3|1|23.7|3|1
16-Mar-16|EPI_43|Time0|Field|Day0|20160912|20161025|20161026|17.2|140|2.408|5.8|1|19.2|3|1
16-Mar-16|EPI_44|Time0|Field|Day0|20160912|20161025|20161026|14.8|140|2.072|6.8|1|18.2|3|1
29-Jul-16|EPI_151|Ambient|Ambient|Day135|20160909|20161004|20161006|11.8|140|1.652|8.5|1|16.5|3|1
29-Jul-16|EPI_152|Ambient|Ambient|Day135|20160909|20161004|20161006|16.8|140|2.352|6|1|19|3|1
29-Jul-16|EPI_153|Ambient|Ambient|Day135|20160909|20161025|20161026|22.4|140|3.136|4.5|1|20.5|3|1
29-Jul-16|EPI_154|Ambient|Ambient|Day135|20160909|20161025|20161026|19.5|140|2.73|5.1|1|19.9|3|1
29-Jul-16|EPI_159|Low|Low|Day135|20160909|20161004|20161006|21.4|140|2.996|4.7|1|20.3|3|1
29-Jul-16|EPI_160|Low|Low|Day135|20160909|20161004|20161006|10.8|140|1.512|9.3|1|15.7|3|1
29-Jul-16|EPI_161|Low|Low|Day135|20160909|20161028|20161028|24.9|140|3.486|4|1|21|3|1
29-Jul-16|EPI_162|Low|Low|Day135|20160909|20161007|20161010|12.5|140|1.75|8|1|17|3|1
29-Jul-16|EPI_167|Super.Low|Super.Low|Day135|20160908|20160930|Pass|13.2|190|2.508|7.6|1|17.4|3|1
29-Jul-16|EPI_168|Super.Low|Super.Low|Day135|20160908|20160930|Pass|6.18|190|1.1742|16.2|1|8.8|3|1
29-Jul-16|EPI_169|Super.Low|Super.Low|Day135|20160908|20161007|20161010|14.8|140|2.072|6.8|1|18.2|3|1
29-Jul-16|EPI_170|Super.Low|Super.Low|Day135|20160908|20161007|20161010|11.2|140|1.568|8.9|1|16.1|3|1
8-Aug-16|EPI_175|Bin1|Low-Ambient|Day145|20160908|20160930|Pass|19.6|190|3.724|5.1|1|19.9|3|1
8-Aug-16|EPI_176|Bin1|Low-Ambient|Day145|20160908|20160930|Pass|7.84|190|1.4896|12.8|1|12.2|3|1
8-Aug-16|EPI_181|Bin2|Ambient-Ambient|Day145|20160908|20161004|20161006|40.8|140|5.712|2.5|1|22.5|3|1
8-Aug-16|EPI_182|Bin2|Ambient-Ambient|Day145|20160908|20161004|20161006|20.8|140|2.912|4.8|1|20.2|3|1
8-Aug-16|EPI_184|Bin3|Ambient-Ambient|Day145|20160908|20161007|20161010|7.46|140|1.0444|13.4|1|11.6|3|1
8-Aug-16|EPI_185|Bin3|Ambient-Ambient|Day145|20160907|20161007|20161010|18.6|140|2.604|5.4|1|19.6|3|1
8-Aug-16|EPI_187|Bin4|Super.Low-Ambient|Day145|20160907|20160930|Pass|17.1|190|3.249|5.8|1|19.2|3|1
8-Aug-16|EPI_188|Bin4|Super.Low-Ambient|Day145|20160907|20160930|Pass|14.5|190|2.755|6.9|1|18.1|3|1
8-Aug-16|EPI_193|Bin5|Low-Ambient|Day145|20160907|20161004|20161006|9.26|140|1.2964|10.8|1|14.2|3|1
8-Aug-16|EPI_194|Bin5|Low-Ambient|Day145|20160907|20161004|20161006|11.7|140|1.638|8.5|1|16.5|3|1
8-Aug-16|EPI_199|Bin6|Super.Low-Ambient|Day145|20160907|20161004|20161006|21.8|140|3.052|4.6|1|20.4|3|1
8-Aug-16|EPI_200|Bin6|Super.Low-Ambient|Day145|20160907|20161004|20161006|21.2|140|2.968|4.7|1|20.3|3|1
8-Aug-16|EPI_205|Bin7|Ambient-Low|Day145|20160907|20160930|Pass|11.9|190|2.261|8.4|1|16.6|3|1
8-Aug-16|EPI_206|Bin7|Ambient-Low|Day145|20160906|20160930|Pass|18.1|190|3.439|5.5|1|19.5|3|1
8-Aug-16|EPI_208|Bin8|Low-Low|Day145|20160906|20161007|20161010|31|140|4.34|3.2|1|21.8|3|1
8-Aug-16|EPI_209|Bin8|Low-Low|Day145|20160906|20161007|20161010|22|140|3.08|4.5|1|20.5|3|1
8-Aug-16|EPI_214|Bin9|Super.Low-Low|Day145|20160906|20161004|20161006|13.3|140|1.862|7.5|1|17.5|3|1
8-Aug-16|EPI_215|Bin9|Super.Low-Low|Day145|20160906|20161004|20161006|22.6|140|3.164|4.4|1|20.6|3|1
8-Aug-16|EPI_220|Bin10|Super.Low-Low|Day145|20160906|20160930|Pass|48.8|190|9.272|2|1|23|3|1
8-Aug-16|EPI_221|Bin10|Super.Low-Low|Day145|20160906|20160930|Pass|30|190|5.7|3.3|1|21.7|3|1
8-Aug-16|EPI_226|Bin11|Ambient-Low|Day145|20160902|20161007|20161010|13.1|140|1.834|7.6|1|17.4|3|1
8-Aug-16|EPI_227|Bin11|Ambient-Low|Day145|20160902|20161007|20161010|31.6|140|4.424|3.2|1|21.8|3|1
8-Aug-16|EPI_229|Bin12|Low-Low|Day145|20160902|20160930|Pass|17.4|190|3.306|5.7|1|19.3|3|1
8-Aug-16|EPI_230|Bin12|Low-Low|Day145|20160902|20160930|Pass|10.1|190|1.919|9.9|1|15.1|3|1

### Step 1 conclusions

* no errors were seen on the PCR machine. Samples assumed to have been cut with MSPI

# Step 2 Bisulfite conversion
* Removed from incubator at 09:00 on 20161215
* Proceeded with EZ DNA Methylation-Gold Kit (Catalog Nos. D5005 & D5006)
[EZ DNA Methylation-Gold Manual](https://github.com/hputnam/Putnam_Lab_Notebook/blob/master/protocols/DNA_Methylation_Gold_Bisulfite_d5005i.pdf)


### Bisulfite conversion kit prep

* Prepared the **CT Conversion Reagent** for larger DNA samples (30µl) by adding 800µl of water, 300µl of M-Dilution and 50µl of M-Dissolving

* Added 30µl of sample described in table to 120 µl of **CT Conversion Reagent**, mixed by flicking, and spun down
* Placed tubes in PCR machine at 10:45 and set as follows:
	* 98°C for 10 min
	* 64°C for 2.5 hours
	* 4°C forever

* followed protocol steps [EZ DNA Methylation-Gold Manual](https://github.com/hputnam/Putnam_Lab_Notebook/blob/master/protocols/DNA_Methylation_Gold_Bisulfite_d5005i.pdf)

* Eluted in 12µl of elution buffer
