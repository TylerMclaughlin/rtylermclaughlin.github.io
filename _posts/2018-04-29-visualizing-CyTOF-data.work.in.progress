---
layout: post
title: "Visualizing high-dimensional CyTOF data"
author: "R. Tyler McLaughlin"
date: "April 29th, 2018"
categories: blog
---

High-dimensional flow cytometry
-------------------------------

All cells in the human body, especially cells of the immune system, rely
on cell surface proteins for cell-cell recognition and decision making.
The display of these surface proteins that interact with one another is
important developmentally. It also underlies how our immune cells
succeed or often fail to target or kill cancerous, infected, or foreign
cells.

Mass cytometry (CyTOF) technology has greatly enhanced the number of
different cell surface proteins that can be measured simultaneously on
single cells. Traditional flow cytometry can only measure 18 markers at
best because of the spectral limitations of antibody-conjugated
fluorescent molecules. CyTOF, on the other hand, replaces the
fluorescent molecules with pure metal element isotopes. Their unique
mass-to-charge ratios permits quantitation of around 40 different
surface proteins. CyTOF can be thought of as a combination of mass
spectrometry and flow cytometry.

CyTOF is capable of analyzing hundreds of thousands of cells at a rate
of 500 cells per second. While more expensive and slower than
traditional flow cytometry, CyTOF has become extremely useful in
immunology, hematology, and oncology.

Because of the widespread use and challenges associated with
interpreting very high-dimensional data, a large number of data analysis
tools have been invented and applied to this new kind of experimental
data.

In this tutorial, I want to show how you can make sense of high-dimensional CyTOF data using a common analysis procedure called SPADE.  I use a publically available dataset from the paper
["Genetic and environmental determinants of human NK cell diversity
revealed by mass
cytometry"](https://www.ncbi.nlm.nih.gov/pubmed/24154599) by Amir
Horowitz et al.  I show how to do the analysis procedure SPADE, which stands for "Spanning-Tree Progression Analysis of Density Normalized Events".

Loading the data
----------------

Let's load up an R `data.table` that includes some of the NK cell
diversity data (the data.table is included in this repository so you can
follow along without creating an account on the Immport hosting
website).

```r
library(data.table)
load('../scripts/CyTOF-data.Rda')
dt1
```

    ##                         .rownames                      name    Time
    ##      1: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs     304
    ##      2: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs     698
    ##      3: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs    1458
    ##      4: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs    1748
    ##      5: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs    1805
    ##     ---                                                            
    ## 225533: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs 1195497
    ## 225534: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs 1195497
    ## 225535: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs 1197389
    ## 225536: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs 1197743
    ## 225537: 050112-HNK-001.400899.fcs 050112-HNK-001.400899.fcs 1198991
    ##         Cell_length CD3(Cd112)Dd Dead(In115)Dd   (La139)Dd CD27(Pr141)Dd
    ##      1:          13   1.10314953   -0.46233210 -0.80646479    -0.1573542
    ##      2:          30  76.48967743   -0.43741027 -0.12476640    -0.8661567
    ##      3:          39 280.68817139   13.68591976 -0.20728706     2.6856558
    ##      4:          15  46.25321198   -0.04494249 -0.03435722     1.0758704
    ##      5:          31  10.47432423   -0.60623127 -0.81096518    -0.6230744
    ##     ---                                                                 
    ## 225533:          12  70.54581451   -0.74006587 -0.43051377    -0.3063238
    ## 225534:          25  17.87489510    0.90569633  8.54550171    -0.8030554
    ## 225535:          14   9.76210880    1.93980765 -0.95653862    -0.6065903
    ## 225536:          29  -0.22724776   -0.30607799  0.01397267    -0.9897838
    ## 225537:          62  -0.01646921    8.23809147 -0.95383924    -0.8463289
    ##         CD19(Nd142)Dd CD4(Nd143)Dd CD8(Nd144)Dd CD57(Nd145)Dd
    ##      1:    -0.5355515  -0.04840276   -0.1690710    -0.7612519
    ##      2:    -0.9283917  -0.40803096   -0.9770941    -0.5209473
    ##      3:     1.2795770  13.51216412  118.8767624     3.4303446
    ##      4:    -0.3512788   5.04316854    0.7875281    45.0088959
    ##      5:     0.6797642   2.04846406   64.1036606     1.5740787
    ##     ---                                                      
    ## 225533:    -0.7396499   9.54289150    7.5340042    -0.4338961
    ## 225534:    -0.6569921  14.45213032    1.4308130    -0.6344429
    ## 225535:    -0.7931423  -0.45907399   -0.9591206    -0.2757547
    ## 225536:    -0.2811626  -0.38778612   41.4754372    42.9413376
    ## 225537:    -0.3158534  -0.07222521   -0.3573135    -0.7396023
    ##         2DL1-S1(Nd146)Dd TRAIL(Sm147)Dd 2DL2-L3-S2(Nd148)Dd CD16(Sm149)Dd
    ##      1:       -0.8581651    -0.86821526         -0.21616583    -0.4744928
    ##      2:       -0.7112357     2.16791415         -0.35066220    13.2766171
    ##      3:       -0.7626939    -0.42648152         -0.26090157    -0.5224886
    ##      4:       -0.8719909    -0.56116188         -0.04321324    -0.2668035
    ##      5:       -0.3581734    -0.05127268         15.94858456   239.3411560
    ##     ---                                                                  
    ## 225533:       -0.9645709    -0.25910035         -0.49259460     6.6512074
    ## 225534:       -0.5242367    -0.65233696         -0.41783011    -0.3760158
    ## 225535:       -0.7066421    -0.04476137         -0.89984703    -0.2851793
    ## 225536:       -0.3735852    -0.63069695          0.72603613   284.2398376
    ## 225537:       -0.2225304    -0.20532438         -0.87298775    -0.1160040
    ##         CD10(Nd150)Dd 3DL1-S1(Eu151)Dd CD117(Sm152)Dd 2DS4(Eu153)Dd
    ##      1:    -0.4114169      -0.85493124    -0.42517659   -0.69061810
    ##      2:    -0.1124230      -0.28226453    -0.96883112   -0.85484636
    ##      3:    -0.4392274      -0.62660253    -0.77438420   -0.61861807
    ##      4:    -0.4572523      -0.28455696    -0.59852690   -0.06368297
    ##      5:     2.8280900       0.72377509    13.34464359   -0.65437567
    ##     ---                                                            
    ## 225533:    -0.3907136      -0.02644462    -0.15441388   -0.94942564
    ## 225534:    -0.2592435      -0.40943614    -0.24768750   -0.43505174
    ## 225535:    -0.6093774      -0.62730718    -0.27563834   -0.73860824
    ## 225536:     9.3106241      -0.46607965     0.38383174   -0.56674433
    ## 225537:    -0.8197008      -0.17904912    -0.08698712   -0.18397211
    ##         ILT2-CD85j(Gd154)Dd NKp46(Gd155)Dd NKG2D(Gd156)Dd NKG2C(Gd157)Dd
    ##      1:          -0.3539338    -0.66441029    -0.96138859     -0.8019629
    ##      2:          -0.8032979    -0.90800095    -0.26815441     -0.1261025
    ##      3:          -0.9215575    -0.09379699    12.94584179     -0.4021138
    ##      4:          -0.5602325     1.16255081     2.60690141     -0.2144874
    ##      5:          -0.2741634    35.74973297    10.76242161      9.3014994
    ##     ---                                                                 
    ## 225533:          -0.6521444    -0.82821190    -0.96219838     -0.3728386
    ## 225534:          -0.8738756    -0.86266655    -0.97669083     -0.3556575
    ## 225535:          -0.2198186    -0.47961900    -0.02651707     -0.5257130
    ## 225536:          -0.1938324     7.24489784     6.58571005     -0.3517002
    ## 225537:          -0.0615587    -0.16593818     2.24241519     -0.6018019
    ##         2B4(Gd158)Dd CD33(Tb159)Dd CD11b(Gd160)Dd NKp30(Dy161)Dd
    ##      1:   -0.2758069    -0.9280193    -0.60748577   -0.003166931
    ##      2:   -0.3362860    -0.7359363    -0.81879216   -0.203208938
    ##      3:   -0.6606493     1.2866890    13.74960327   -0.647054613
    ##      4:   11.3935432     1.9085668    -0.39056683    2.239806175
    ##      5:   15.3896828     1.3614255    10.13512230   16.471530914
    ##     ---                                                         
    ## 225533:   -0.4594081    36.7935028    -0.30501413   -0.556638241
    ## 225534:   -0.5936617     1.1505328    -0.35569543   -0.234679982
    ## 225535:   -0.0661846    -0.6217228    -0.49344796   -0.762706161
    ## 225536:   13.1160126     8.8597393     3.87789416    8.075531006
    ## 225537:   -0.2530261    54.4277000    -0.03413384   -0.038968075
    ##         CD122(Dy162)Dd 3DL1(Dy163)Dd NKp44(Dy164)Dd CD127(Ho165)Dd
    ##      1:     -0.2493532    -0.2673971   -0.922300279     -0.8327850
    ##      2:     -0.7723597    -0.8742337   -0.005917197     -0.3164448
    ##      3:     -0.4768949    -0.3273244   -0.273348629     -0.5167781
    ##      4:     -0.6738622    -0.1137003   -0.258331150     -0.6021543
    ##      5:     12.9986467    78.4009552    6.062179565     -0.3706971
    ##     ---                                                           
    ## 225533:     -0.4436572    -0.7629591   -0.101451308     -0.8723339
    ## 225534:     -0.3234364    -0.6304750   -0.022156846     -0.5030025
    ## 225535:     -0.8514883    -0.3788523   -0.107373759     -0.0751335
    ## 225536:      1.1761544    -0.5235777   -0.785984159     -0.2715366
    ## 225537:     28.6753616    -0.9050320   -0.226699620     -0.6271428
    ##         2DL1(Er166)Dd CD94(Er167)Dd CD34(Er168)Dd CCR7(Tm169)Dd
    ##      1:   -0.02531562    1.96062708   -0.66500634   -0.44380611
    ##      2:   -0.80763638   -0.46083874   -0.18503122   -0.87376732
    ##      3:   -0.47822216   -0.37212703   -0.66628766   11.41296101
    ##      4:   -0.35252780   -0.00711298   -0.03269264   -0.04186028
    ##      5:   32.06984329    5.15235662   -0.83730745    1.28816688
    ##     ---                                                        
    ## 225533:   -0.58442903   -0.60887551    1.97557962   -0.16915819
    ## 225534:   -0.01272007   -0.03078870   -0.88963079   10.43365860
    ## 225535:   -0.13549404   -0.58453619   -0.12029273   -0.07600144
    ## 225536:   -0.48636356   88.66054535   34.40827179   -0.94932491
    ## 225537:   -0.30052346   -0.05932372   -0.42091009   10.81522369
    ##         2DL3(Er170)Dd NKG2A(Yb171)Dd HLA-DR(Yb172)Dd 2DL4(Yb173)Dd
    ##      1:    -0.6002591     -0.4054997     -0.53848648    -0.6175402
    ##      2:    -0.3960588     -0.8390744      4.23451376     1.5093910
    ##      3:    -0.4087883     -0.1808962     -0.02444823    -0.4218564
    ##      4:    -0.6677604     -0.5194318     -0.91385984    -0.6683684
    ##      5:   109.9773941     -0.1536108     -0.08078133     1.5147980
    ##     ---                                                           
    ## 225533:    -0.7183176     -0.3685567     -0.52344221    -0.1416938
    ## 225534:    -0.9971635     -0.1518698      7.66569757    -0.1121935
    ## 225535:    -0.7616016     -0.8233932     -0.35577059    -0.7892748
    ## 225536:    -0.8551161    102.5373230      7.16106319     5.6843553
    ## 225537:    -0.4232238     -0.7688891      2.13248205    -0.3783518
    ##         CD56(Yb174)Dd 2DL5(Lu175)Dd CD25(Yb176)Dd DNA1(Ir191)Dd
    ##      1:    -0.2297794   -0.16890252   -0.79064298     170.43468
    ##      2:    -0.4827193   -0.13701023   -0.32096153     296.14062
    ##      3:    -0.6115051   -0.88886482   -0.05193671     480.61362
    ##      4:    -0.2160397   -0.72292501   -0.12561615     101.62951
    ##      5:    -0.5796017    4.86288071   -0.35362631     944.83783
    ##     ---                                                        
    ## 225533:    -0.9186369   -0.76251358   -0.26187009      21.03497
    ## 225534:    -0.8505155   -0.75358284   -0.78666514     295.78491
    ## 225535:    -0.3991568   -0.62808973   -0.53029406      74.82860
    ## 225536:    64.4925766   -0.77292234    0.42967427     174.00893
    ## 225537:    -0.1682667   -0.07813975   -0.50799221    1032.54871
    ##         DNA2(Ir193)Dd
    ##      1:     141.03711
    ##      2:     468.05478
    ##      3:     930.11731
    ##      4:     164.93912
    ##      5:    1287.63806
    ##     ---              
    ## 225533:      39.77903
    ## 225534:     722.46771
    ## 225535:     219.93697
    ## 225536:     230.41719
    ## 225537:    1265.57129

### Installing spade package in R

This package is from [Gary Nolan's research group](http://web.stanford.edu/group/nolan/) at Stanford.

```r
install.packages("devtools")
library(devtools)
devtools::install_github("nolanlab/Rclusterpp")
source("http://bioconductor.org/biocLite.R")
devtools::install_github("nolanlab/spade")
```

Note:  do not install Rclusterpp using CRAN (`install.packages("Rclusterpp")`) because an outdated version is hosted on CRAN that is not compatible with the newer version of SPADE.

If you're having trouble installing Rclusterpp via github (as I did) because of OpenMP, follow [this guide](http://thecoatlessprofessor.com/programming/openmp-in-r-on-os-x/#after-3-4-0).

Now that we have set up the package and its dependencies, let's try running the algorithm:

```r
library(spade)
SPADE.driver('.')
```


The first part of SPADE is downsampling.
This outputs 

Performing vi-T-SNE
-------------------
COMING SOON


Conclusions
-----------


Visualizing CyTOF data in this blog post. I'd like to thank Emily Mace
for introducing me to the technology.
