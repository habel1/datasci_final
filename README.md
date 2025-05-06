Let Trans Lives Thrive
================
Zoe Penzarro Habel
May 6th, 2025

As an undergraduate researcher, I’ve had the privilege of exploring and
familiarizing myself with a few public-use datasets and assisting my
professors with various parts of the research process. One data set I’ve
worked with several times is
[**TransPop**](https://doi.org/10.3886/ICPSR37938.v1), a nationally
representative survey on transgender health which includes data from 274
transgender and gender nonconforming (TGNC) adults as well as and 1,162
cisgender adults conducted between 2016-2018 (Meyer 2021).

The recent attacks on TGNC rights have been on my mind a lot, and
putting my energy into research helps me feel a bit more in control.
When I got this assignment, I knew I wanted to look at TransPop again
because I knew there was so much more to investigate. What I didn’t
expect was the new banner which greeted me at the top of the research
repository:

> On March 31, 2025, as a sponsor of this project, NIH requested that
> the following language be added to this website: This repository is
> under review for potential modification in compliance with
> Administration directives.

##### I cannot find the words to adequately express how angry I am about the politicization of parts of the human experience as beautiful as transness and the queer community. In lieu of an eloquent statement, I’d like to offer some hope.

## **Project Background + Research Aims**

For this brief project, I decided to investigate the impact of stressors
specific to the trans community on the social well-being and
satisfaction with life of TGNC adults. By identifying patterns in the
data, I endeavor to uncover what makes a TGNC person more likely to
thrive, and thus, how we can help transgender lives flourish.

### ***Question: Which experiences and stressors (or the lack thereof) are most pertinent to the social well-being and satisfaction with life of TGNC adults?***

## **Procedure**

I chose to utilize a conditional inference tree model for my analysis.
Conditional inference trees offer a robust method for regression
analysis in decision trees and work well with multivariate analysis.

13 variables were included as potential predictors in the conditional
inference tree models. Four total models were fitted; one tree with only
the 4 demographic variables included as predictors for both outcome
measures, and one tree with the 9 remaining variables included as
predictors for both outcome measures.

**For detailed information on the measures and survey items as well as
full citations for the subscales included in this paper, please see the
official methods and technical notes document for the TransPop survey
(Krueger et al., 2020).**

### **Demographics**

**Age, race, gender-identity, and sexual minority identity were included
as demographic variables.** Age was measured in years. For race, the
variable in which responses were recoded as White,
Black,Latino/Hispanic/Spanish origin, Multirace, and “other” was used
for analysis (race_recode_cat5). Since the data from cisgender adults is
not included in analysis for this paper, gender identity includes trans
man (FTM), trans woman (MTF), or transgender gender nonbinary. Sexual
minority identity is a binary label, with 0 representing hetereosexual
and 1 representing any sexual minority identity labels (homosexual,
bisexual etc).

### **Measures**

#### ***Outcome variables***

I chose social well-being and satisfaction with life as my two outcome
measures. Only complete cases were included for these composite scores.

**Social Well-Being**  
This 15-item scale (Keyes, 1998) evaluates individuals’ perceived social
wellness, including their sense of community belonging, comfort, and
societal contribution. Participants rate statements like *“I don’t feel
I belong to anything I’d call a community”* (reverse-coded) and *“My
community is a source of comfort”* on a 7-point Likert scale (1 =
*strongly disagree*, 7 = *strongly agree*). Eight items are
reverse-coded before calculating a mean score (range: 1–7), with higher
scores reflecting stronger social well-being—greater feelings of
connection, support, and purpose in society.

**Satisfaction with Life Scale (SWLS)**  
The SWLS (Diener et al., 1985) measures global life satisfaction through
five items (e.g., *“In most ways my life is close to ideal”* and *“I am
satisfied with life”*), rated on a 7-point Likert scale (1 = *strongly
disagree*, 7 = *strongly agree*). A mean score (range: 1–7) is computed,
where higher scores indicate greater life satisfaction—more positive
judgments about one’s life overall.

#### ***Predictors***

Each of the 9 variables included as predictors in this analysis are
*composite scores* calculated from empirically validated scales included
as survey items in the original questionnaire. **Missing data was
replaced with predicted mean matching imputation,** with both imputed
and non-imputed versions of the composite scores available for analysis
within the dataset. **I chose to use the imputed composite scores as
predictors in my analysis.** The predictors included were categorized as
**identity, healthcare access & utilization, or stressors** by the study
authors.

1.  **Identity:**
    - **Community Connectedness**: A 5-item scale (1–5) assessing
      transgender community affiliation; higher mean scores indicate
      stronger connectedness.  
    - **Non-Affirmation of Gender Identity**: A 6-item scale (1–5)
      measuring perceived lack of understanding/acceptance; higher mean
      scores indicate greater non-affirmation.
2.  **Healthcare access & utilization:**
    - **Healthcare Stereotype Threat**: A 4-item scale (1–5) assessing
      fear of being judged by providers due to LGBT identity; higher
      mean scores indicate greater worry.
3.  **Stressors:**
    - **Internalized Transphobia**: A 6-item scale (1–5) measuring
      self-directed stigma; higher mean scores indicate stronger
      internalized transphobia.  
    - **Everyday Discrimination**: A 9-item scale (1–4, reverse-coded)
      assessing chronic discrimination; higher mean scores indicate more
      frequent experiences.  
    - **Childhood Gender Conformity**: A 4-item scale (1–5) categorizing
      participants (based on sex at birth) into deciles of gender
      nonconformity (1 = most GNC, 3 = least GNC).  
    - **Gender Identity Non-Disclosure**: A 5-item scale (1–5) measuring
      avoidance of disclosing gender identity; higher mean scores
      indicate greater concealment efforts.  
      -**Negative Expectations of the Future**: A 9-item scale (1–5)
      assessing anticipated rejection due to gender identity; higher
      mean scores indicate more pessimistic expectations.  
    - **Social Support**: A 12-item scale (1–7) measuring perceived
      support; higher mean scores indicate stronger social support.

## **Analysis**

First, I loaded the packages `partykit` and `tidyverse` for fitting my
conditional inference tree models and data wrangling respectively.

I then used the `read.delim` function from the `readr` subpackage to
load the tab seperated values file containing the data downloaded from
the [ICPSR repository](https://doi.org/10.3886/ICPSR37938.v1).

``` r
transpop <- read.delim("37938-0005-Data.tsv")
```

Here’s a quick preview of the raw data:

``` r
head(transpop)
```

    ##     STUDYID WEIGHT_CISGENDER_TRANSPOP WEIGHT_CISGENDER WEIGHT_TRANSPOP
    ## 1 151768927               0.022039215               NA       0.9861429
    ## 2 152357242               0.008485489               NA       0.3796825
    ## 3 152444055               0.015764496               NA       0.7053812
    ## 4 152525272               0.035655390               NA       1.5953976
    ## 5 152894493               0.041801889               NA       1.8704222
    ## 6 152925625               0.021335387               NA       0.9546502
    ##   GMETHOD_TYPE SURVEYCOMPLETED GRESPONDENT_DATE GCENREG RACE RACE_RECODE
    ## 1                            0      26-APR-2016       1    6           1
    ## 2                            0      07-APR-2016       3    6           1
    ## 3                            0      01-MAY-2016       3    6           1
    ## 4                            0      20-APR-2016       4    6           1
    ## 5                            0      05-MAY-2016       2    8           3
    ## 6                            0      10-JUN-2016       1    8           6
    ##   RACE_RECODE_CAT5 SEXUALID SEXMINID HINC HINC_I PINC PINC_I GEDUC1 GEDUC2
    ## 1                1        1        0   11     11    4      4      2      2
    ## 2                1        1        0    7      7    7      7      3      2
    ## 3                1        5        1    9      9    4      4      2      2
    ## 4                1        3        1   11     11    1      1      1      1
    ## 5                3        1        0    8      8    8      8      2      2
    ## 6                4        1        0    4      4    4      4      2      2
    ##   GANN_INC GANN_INC2 GD74 GD75 GD76 GEDUCATION GEMPLOYMENT2010        GMSANAME
    ## 1        7        NA    1    1   NA          4               3 MASKED BY ICPSR
    ## 2        4        NA    2    1   NA          5               5 MASKED BY ICPSR
    ## 3        7        NA    2    1   NA          3               3 MASKED BY ICPSR
    ## 4        9        NA    1    1   NA          1               4 MASKED BY ICPSR
    ## 5        6        NA    2    1   NA          4               1 MASKED BY ICPSR
    ## 6        4        NA    2    1   NA          4               6 MASKED BY ICPSR
    ##   SEX GENDER_IDENTITY TRANS TRANS_CIS AGE GURBAN GURBAN_I POVERTY POVERTY_I
    ## 1   2               4     2         1  65      1        1       0         0
    ## 2   1               3     1         1  38      1        1       0         0
    ## 3   1               3     1         1  25      1        1       0         0
    ## 4   1               3     1         1  18      1        1       0         0
    ## 5   1               3     1         1  30      1        1       0         0
    ## 6   1               3     1         1  64      0        0       0         0
    ##   POVERTYCAT POVERTYCAT_I GRACE GRACE_I GD25A GD65 GD66 GD66A GP20 GD1 GD15C
    ## 1          4            4     1      NA    NA   NA   NA    NA   NA  NA    NA
    ## 2          3            3     1      NA    NA   NA   NA    NA   NA  NA    NA
    ## 3          4            4     1      NA    NA   NA   NA    NA   NA  NA    NA
    ## 4          4            4     1      NA    NA   NA   NA    NA   NA  NA    NA
    ## 5          4            4     5      NA    NA   NA   NA    NA   NA  NA    NA
    ## 6          2            2     2      NA    NA   NA   NA    NA   NA  NA    NA
    ##   GD4_1 GD5 GD63 GD69 GD69_1 GD69_2 GD69_3 GD69_4 GD69_5 GD7 GD8B GD9 GP1 GP2
    ## 1    NA  NA   NA   NA     NA     NA     NA     NA     NA  NA   NA  NA  NA  NA
    ## 2    NA  NA   NA   NA     NA     NA     NA     NA     NA  NA   NA  NA  NA  NA
    ## 3    NA  NA   NA   NA     NA     NA     NA     NA     NA  NA   NA  NA  NA  NA
    ## 4    NA  NA   NA   NA     NA     NA     NA     NA     NA  NA   NA  NA  NA  NA
    ## 5    NA  NA   NA   NA     NA     NA     NA     NA     NA  NA   NA  NA  NA  NA
    ## 6    NA  NA   NA   NA     NA     NA     NA     NA     NA  NA   NA  NA  NA  NA
    ##   GSURVEY GWP10200 GWP10202 GWP10208 GWP10209 GWP10215 GWP10216 GWP119 GWP1223
    ## 1      NA       NA       NA       NA       NA       NA       NA     NA      NA
    ## 2      NA       NA       NA       NA       NA       NA       NA     NA      NA
    ## 3      NA       NA       NA       NA       NA       NA       NA     NA      NA
    ## 4      NA       NA       NA       NA       NA       NA       NA     NA      NA
    ## 5      NA       NA       NA       NA       NA       NA       NA     NA      NA
    ## 6      NA       NA       NA       NA       NA       NA       NA     NA      NA
    ##   GWP1225 GRUCA GRUCA_I GZIPCODE GZIPSTATE GMILESAWAY GCENDIV Q01 Q02 Q03 Q04
    ## 1      NA   1.0     1.0        1         1  65.501849       1   5   7   3   2
    ## 2      NA   1.0     1.0        1         1  68.697748       5   8   9   2   5
    ## 3      NA   1.0     1.0        1         1   4.661613       5   7   8   2   2
    ## 4      NA   1.0     1.0        1         1 203.235812       8   4   5   3   2
    ## 5      NA   1.0     1.0        1         1  30.665798       3   8  10   2   4
    ## 6      NA  10.1    10.1        1         1  33.381687       1   8   8   2   1
    ##   Q05 Q06 Q07 Q08 Q09 Q10 Q11 Q12 Q13 Q14 Q15 Q16 Q17 Q18 Q19A Q19B Q19C Q19D
    ## 1   6   6   5   6   5   6   5   3   5   3   3   2   2   2    1    1    1    1
    ## 2   3   3   4   5   5   6   2   1   5   3   3   3   6   2    1    1    1    1
    ## 3   5   5   3   5   7   6   3   1   1   7   7   1   5   4    1    1    1    1
    ## 4   2   2   2   3   4   1   6   7   1   3   3   4   4   4    2    2    2    2
    ## 5   5   4   2   6   5   7   1   1   3   6   7   2   4   5    1    1    2    1
    ## 6   7   6   4   6   7   7   4   1   4   1   1   1   1   1    2    1    1    2
    ##   Q20_1 Q20_2 Q20_3 Q20_4 Q20_5 Q20_6 Q20_7 Q21 Q22 Q23 Q24 Q25 Q26 Q27 Q28 Q29
    ## 1    NA    NA    NA    NA    NA     6    NA       2   2   2   2   2   2  NA  NA
    ## 2    NA    NA    NA    NA    NA     6    NA       1   2   4   2   2   1  NA  NA
    ## 3    NA    NA    NA    NA    NA     6    NA       1   3   3   3   3   3  NA  NA
    ## 4    NA    NA    NA    NA    NA     6    NA       4   3   5   4   4   2  NA  NA
    ## 5    NA     2     3    NA    NA    NA    NA       5   5   5   5   5   4  NA  NA
    ## 6    NA    NA    NA    NA    NA     6     7       3   4   3   3   3   3  NA  NA
    ##   Q30 Q31 Q32 Q33 Q34 Q34_VERB Q35_1 Q35_2 Q35_3 Q35_4 Q35_5 Q36A Q36B Q36C
    ## 1   7       2   1   1              1    NA    NA    NA    NA    4    2    8
    ## 2   7       2   2   1              1    NA    NA    NA    NA    4    1    2
    ## 3   7      NA   4   5              1     2    NA     4    NA    4    4    1
    ## 4   7       2   2   3             NA    NA    NA    NA     5    1    4    1
    ## 5   7       2   3   1              1    NA    NA    NA    NA    4    1    2
    ## 6   7       2   3   1              1    NA    NA    NA    NA    4    1    8
    ##   Q36D Q36E Q36F Q37 Q38 Q39 Q40 Q41 Q42 Q43 Q44 Q45 Q46 Q47 Q48 Q49 Q50 Q51
    ## 1    2    8    8   1  36   1   1   4   3   3   5   3   3   3   3   3   4   4
    ## 2    1    1    1   1   5   1   1   1   7   6   5   1   1   1   1   1   2   3
    ## 3    3    3    4   1   3   1   1   4   6   6   5   2   1   2   2   3   4   4
    ## 4    4    3    3   2  97   7   7   7   5   4   4   5   5   5   5   5   5   2
    ## 5    2    3    1   2  97   7   7   7   7   7   4   1   1   1   1   1   1   3
    ## 6    1    1    1   1   9   1   2   4   7   6   5   2   1   1   1   1   1   4
    ##   Q52 Q53 Q54 Q55 Q56 Q57 Q58 Q59 Q60 Q61 Q62_T_VERB Q62A Q62B Q62C Q62D Q62E
    ## 1   4   4   4   2   4   7  19   5   3   3               7    7    7    7    7
    ## 2   4   4   2   2   5  13  22   2   1   1               1    1    2    4   NA
    ## 3   3   2   4   4  12  14  21   2   2   2               1    2    4    4    4
    ## 4   2   5   3   3  10  12  17   4   3   3               2    2    2    2    3
    ## 5   3   4   3   2   5  14  18   2   1   1               1    3    3    3   NA
    ## 6   4   4   2   2   7  10  21   2   1   1               1    1    2    2    4
    ##   Q63_T_VERB Q63A Q63B Q63C Q63D Q63E Q63F Q63G Q63H Q63I Q63J Q64 Q65 Q66 Q67
    ## 1               2    2    3    3    3    3    2    2    2    2 997   2  97   7
    ## 2               7    7    7    7    7    7    7    7    7    7  30   1  22   1
    ## 3               7    7    7    7    7    7    7    7    7    7  23   1  20   1
    ## 4               7    7    7    7    7    7    7    7    7    7 997   2  97   7
    ## 5               7    7    7    7    7    7    7    7    7    7  28   1  14   1
    ## 6               7    7    7    7    7    7    7    7    7    7  53   2  97   7
    ##   Q68 Q69 Q70 Q71 Q72 Q73_1 Q73_2 Q74 Q75 Q76_1 Q76_2 Q77 Q78 Q79 Q80 Q81_1
    ## 1   7   2  97   7  97     7     7   2  97     7     7   2   2   2   2    NA
    ## 2   1   1  27  NA  97    NA    NA   1  26    NA    NA   4   4   4   4    NA
    ## 3   2   1  22  NA  97    NA    NA   1  21    NA    NA   4   5   5   5    NA
    ## 4   7   1  NA  NA  97    NA    NA   2  97     7     7   5   5   5   5    NA
    ## 5   1   1  27  NA  97    NA    NA   2  97     7     7   2   2   2   2    NA
    ## 6   7   1  NA  NA  97    NA    NA   2  97     7     7   1   1   1   1    NA
    ##   Q81_2 Q81_3 Q81_4 Q81_5 Q81_6 Q81_7 Q81_8 Q81_9 Q81_10 Q81_11 Q81_12 Q81_13
    ## 1     2    NA    NA    NA    NA     7     8    NA     NA     NA     NA     NA
    ## 2    NA    NA    NA    NA    NA     7    NA    NA     NA     NA     NA     NA
    ## 3    NA    NA    NA    NA    NA    NA    NA    NA     NA     11     NA     NA
    ## 4    NA    NA     4    NA    NA    NA    NA    NA     NA     NA     NA     NA
    ## 5    NA    NA    NA    NA    NA    NA    NA    NA     NA     11     NA     NA
    ## 6    NA    NA    NA    NA    NA    NA    NA     9     NA     NA     NA     NA
    ##   Q81_T_VERB Q82 Q83_1 Q83_2 Q83_3 Q83_4 Q83_5 Q83_T_VERB Q84 Q85 Q86_VERB Q86
    ## 1             NA     7     7     7     7     7              7   1           NA
    ## 2             NA     7     7     7     7     7              7   1           NA
    ## 3             NA     7     7     7     7     7              7   1           NA
    ## 4             NA     7     7     7     7     7              7   2           97
    ## 5             NA     7     7     7     7     7              7   1           NA
    ## 6             NA     7     7     7     7     7              7   1           NA
    ##   Q87 Q88 Q89 Q90_1 Q90_2 Q90_3 Q91 Q92A Q92B Q92C Q92D Q93 Q94 Q95 Q96 Q97_1
    ## 1  NA   2  NA     1    NA    NA  NA   NA    2   NA    1   3   1   1   1    NA
    ## 2  NA   2  NA    NA    NA     3  NA    2    1    2    1   4   1  31  22    NA
    ## 3  NA   1  NA    NA     2     3  NA    1    1    1    1   4   1  17   7    NA
    ## 4  NA   2  NA    NA     2     3  NA    1    1    2    2   3   1  31  21    NA
    ## 5  NA   2  NA     1    NA    NA  NA    1    1    2    1   4   7  11   6     1
    ## 6  NA   2  NA    NA     2    NA  NA    2    2    2    1   4  99  99  99    NA
    ##   Q97_2 Q97_3 Q97_4 Q97_5 Q97_6 Q97_7 Q97_8 Q97_9 Q97_10 Q97_11 Q97_12 Q97_13
    ## 1    NA    NA    NA    NA    NA    NA    NA     9     10     NA     NA     NA
    ## 2    NA    NA    NA    NA    NA    NA    NA    NA     NA     NA     12     NA
    ## 3    NA    NA    NA    NA    NA    NA    NA    NA     NA     NA     NA     NA
    ## 4    NA    NA    NA    NA    NA    NA    NA    NA     NA     NA     NA     13
    ## 5    NA    NA    NA    NA    NA    NA    NA    NA     NA     NA     12     NA
    ## 6    NA    NA    NA    NA    NA    NA    NA    NA     NA     NA     NA     NA
    ##   Q97_14 Q97_15 Q97_16 Q97_17 Q97_18 Q97_19 Q97_20 Q97_21 Q97_22 Q97_23 Q98 Q99
    ## 1     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA   2   2
    ## 2     NA     NA     16     NA     NA     NA     NA     NA     NA     NA   2   2
    ## 3     NA     NA     NA     NA     NA     NA     NA     NA     NA     23   1   1
    ## 4     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA   1   2
    ## 5     NA     NA     NA     NA     NA     NA     NA     NA     NA     23   2   2
    ## 6     NA     NA     16     NA     NA     NA     NA     NA     NA     NA   2   2
    ##   Q100A Q100B Q100C Q100D Q100E Q100F Q101 Q102 Q103 Q104 Q105 Q106 Q107 Q108
    ## 1     5     5     5     5     5     4    2    2    2    2    6    6    5    2
    ## 2     3     5     3     5     4     5    2    2    2    2    6    5    1    1
    ## 3     2     4     2     4     2     5    1    1    1    1    2    3    5    1
    ## 4     2     1     2     2     1     2    2    1    2    1    6    6    5    2
    ## 5     4     5     2     4     2     5    1    2    2    1    2    3    1    1
    ## 6     5     5     5     5     5     5    2    2    2    2    5    6    1    6
    ##   Q109 Q110 Q111 Q112 Q113 Q114 Q115 Q116 Q117 Q118 Q119 Q120 Q121 Q122 Q123
    ## 1    2    1    5    5    2    2    1    3    4    1    2    2    1    1    2
    ## 2    2    1    5    2    0    1    1    3    1    1    1    1    1    1    1
    ## 3    2    2    3    5    1    3    1    2    4    1    2    1    1    1    1
    ## 4    2    1    5    1    0    1    2    7    1    1    1    1    1    1    1
    ## 5    2    1    5    1    0    1    2    7    1    1    1    1    1    1    1
    ## 6    2    1    3    2    0    1    1    3    1    1    1    1    1    1    1
    ##   Q124 Q125 Q126 Q127 Q128 Q129 Q130 Q131 Q132 Q133 Q134 Q135 Q136 Q137 Q138
    ## 1    1    1    1    1    1   97   97   97    1   97   97   97    1   97   97
    ## 2    1    1    1    1    3   97   NA   NA    2   NA   97   97    2   NA   97
    ## 3    1    1    1    1    3   97   NA   NA    3   97   NA   NA    3   97   12
    ## 4    1    1    1    1    3   97   NA   NA    3   97   NA   NA    3   97    9
    ## 5    1    1    1    1    2   NA   97   97    1   97   97   97    1   97   97
    ## 6    1    1    1    1    3   97   NA   NA    3   97   NA   NA    3   97   21
    ##   Q139 Q140 Q141 Q142 Q143 Q144 Q145 Q146 Q147 Q148 Q149 Q150A Q150B Q150C
    ## 1   97    1   97   97   97   97    7    1   97   97   97     2     2    NA
    ## 2   97    1   97   97   97   97    7    1   97   97   97     1     1     1
    ## 3   25    3    5   97   19   20    3    3   97   14   23     1     1     1
    ## 4   18    1   97   97   97   97    7    1   97   97   97     1     1     1
    ## 5   97    1   97   97   97   97    7    2   14   97   97     1     1     1
    ## 6   50    3    1   97   NA   NA    1    1   97   97   97     1     1     1
    ##   Q150D Q150E Q150F Q151 Q152 Q153 Q154 Q155 Q156 Q157 Q158 Q159 Q160 Q161
    ## 1     2     2     2   NA    3    4    3    3    2    2    2    2    2    3
    ## 2     1     2     2    4    2    3    4    2    1    2    3    5    2    4
    ## 3     1     1     1    5    4    4    2    4    3    4    2    4    2    1
    ## 4     1     2     2    3    3    4    4    4    4    4    4    3    4    5
    ## 5     1     2     2    4    4    4    2    2    2    1    2    2    2   NA
    ## 6     1     2     2    2    2    2    2    2    2    2    2    2    2    3
    ##   Q162A Q162B Q162C Q162D Q162E Q162F Q163_1 Q163_2 Q163_3 Q163_4 Q163_5 Q163_6
    ## 1     1     3     2     2     2     1     NA     NA     NA     NA     NA     NA
    ## 2     1     3     2     2     4     4     NA     NA     NA      4     NA     NA
    ## 3     4     1     1     3     4     4     NA      2      3      4     NA     NA
    ## 4     1     1     1     1     1     1      7      7      7      7      7      7
    ## 5     1     2     1     2     3     1      1      2     NA     NA     NA     NA
    ## 6     1     1     1     2     3     1     NA     NA      3      4     NA     NA
    ##   Q163_7 Q163_8 Q163_9 Q163_10 Q164 Q165 Q166_1 Q166_2 Q166_3 Q166_4 Q166_5
    ## 1     NA     NA     NA      NA    4    3     NA     NA     NA     NA     NA
    ## 2     NA      8     NA      NA    2    1      7      7      7      7      7
    ## 3      7     NA     NA      10    3    2     NA      2      3     NA     NA
    ## 4     97     97     97      97    4    1      7      7      7      7      7
    ## 5     NA     NA     NA      NA    1    1      7      7      7      7      7
    ## 6      7     NA     NA      NA    1    3      7      7      7      7      7
    ##   Q166_6 Q166_7 Q166_8 Q166_9 Q166_10 Q167 Q168_1 Q168_2 Q168_3 Q168_4 Q168_5
    ## 1     NA     NA     NA     NA      NA    1      7      7      7      7      7
    ## 2      7     97     97     97      97    1      7      7      7      7      7
    ## 3      6      7     NA     NA      10    1      7      7      7      7      7
    ## 4      7     97     97     97      97    1      7      7      7      7      7
    ## 5      7     97     97     97      97    1      7      7      7      7      7
    ## 6      7     97     97     97      97    1      7      7      7      7      7
    ##   Q168_6 Q168_7 Q168_8 Q168_9 Q168_10 Q169A Q169B Q169C Q169D Q169E Q169F Q169G
    ## 1      7     97     97     97      97     2     2     2     2     2     2     2
    ## 2      7     97     97     97      97     2     2     2     2     2     2     2
    ## 3      7     97     97     97      97     1     2     1     1     1     2     2
    ## 4      7     97     97     97      97     2     2     1     2     2     2     1
    ## 5      7     97     97     97      97     2     2     2     1     1     2     2
    ## 6      7     97     97     97      97     2     2     2     2     2     2     2
    ##   Q169H Q169I Q169J Q169K Q170_1 Q170_2 Q170_3 Q170_4 Q170_5 Q170_6 Q170_7
    ## 1     2     2     2     2      7      7      7      7      7      7     97
    ## 2     2     2     2     2      7      7      7      7      7      7     97
    ## 3     1     2     2     2      1      2      3      4     NA      6      7
    ## 4     2     2     2     2      1     NA      3      4     NA      6     NA
    ## 5     2     2     2     2      1     NA     NA     NA     NA     NA     NA
    ## 6     2     2     2     2      7      7      7      7      7      7     97
    ##   Q170_8 Q170_9 Q170_10 Q171A Q171B Q171C Q171D Q171E Q171F Q171G Q171H Q171I
    ## 1     97     97      97     3     3     2     3     2     3     3     3     4
    ## 2     97     97      97     3     3     4     4     4     4     3     4     4
    ## 3     NA     NA      10     2     2     3     1     2     2     1     2     2
    ## 4     NA     NA      10     2     2     3     2     2     3     2     3     4
    ## 5     NA     NA      NA     2     2     2     2     2     2     2     3     3
    ## 6     97     97      97     4     3     4     3     3     4     2     4     4
    ##   Q172_1 Q172_2 Q172_3 Q172_4 Q172_5 Q172_6 Q172_7 Q172_8 Q172_9 Q172_10 Q173A
    ## 1     NA     NA      3     NA     NA     NA     NA     NA     NA      NA     1
    ## 2     NA     NA      3     NA     NA     NA     NA     NA     NA      NA     0
    ## 3      1     NA     NA     NA     NA      6     NA     NA     NA      NA     2
    ## 4     NA     NA      3      4     NA     NA      7     NA     NA      NA     2
    ## 5     NA     NA     NA     NA      5     NA     NA     NA     NA      NA     1
    ## 6     NA     NA     NA     NA     NA      6     NA     NA     NA      NA     0
    ##   Q173B Q173C Q173D Q173E Q173F Q173G Q173H Q173I Q173J Q173K Q173L Q174 Q175
    ## 1     1     1     1     1     0     0     0     0     0     0     0    3    4
    ## 2     0     1     0     1     0     1     0     0     0     1     0    2    1
    ## 3     2     2     0     0     2     0     0     2     2     0     0    2    6
    ## 4     1     0     2     0     0     1     2     1     2     0     0    2    2
    ## 5     0     2     0     0     0     1     1     1     0     0     0    2    2
    ## 6     0     0     0     0     0     2     0     0     0     0     0    2    1
    ##   Q176 Q177 Q178 Q179 Q180 Q181 Q182 Q183 Q184 Q185 Q186 Q187 Q188 Q189 Q190_1
    ## 1    4    4    2    2    2    2    2    1    1    2    1    1    1    3     NA
    ## 2    1    1    1    2    2    2    2    1    1    1    1    1    1    2     NA
    ## 3    2    3    1    2    2    2    2    1    1    1    1    1    1    1     NA
    ## 4    2    2    1    1    1    1    1    8    1    3    1    1    1    2     NA
    ## 5    2    1    2    2    2    2    2    1    1    3    1    1    1    4      7
    ## 6    2    2    2    2    2    2    2    1    1    1    1    2    1    3      1
    ##   Q190_2 Q190_3 Q190_4 Q190_5 Q190_6 Q190_7 Q190_8 Q190_9 Q190_10 Q191A Q191B
    ## 1     NA     NA     NA     NA     NA     NA     NA     NA      NA     2     4
    ## 2     NA     NA      4     NA     NA     NA      8     NA      NA     4     4
    ## 3      2     NA     NA     NA      6      7     NA     NA      NA     5     5
    ## 4      2     NA     NA     NA     NA     NA      8     NA      NA     3     3
    ## 5      7      7      7      7      7     97     97     97      97     2     2
    ## 6      2     NA     NA     NA      6     NA     NA     NA      NA     1     2
    ##   Q191C Q191D Q191E Q191F Q191G Q191H Q191I Q192A Q192B Q192C Q192D Q192E Q192F
    ## 1     2     3     2     2     4     2     2     6     6     5     4     5     6
    ## 2     4     4     4     4     4     4     4     7     7     7     7     7     7
    ## 3     5     5     5     5     5     5     5     7     7     4     1     7     7
    ## 4     4     4     5     5     4     3     4     6     6     3     2     6     7
    ## 5     3     3     3    NA     3     3     1     7     7     7     7     7     2
    ## 6     1     1     1     1     2     1     1     7     7     7     3     7     7
    ##   Q192G Q192H Q192I Q192J Q192K Q192L Q193_1 Q193_2 Q193_3 Q193_4 Q193_5 Q193_6
    ## 1     6     5     6     6     4     6      1      2      3      4     NA     NA
    ## 2     7     7     7     7     7     7      1      2      3     NA     NA     NA
    ## 3     7     1     7     7     1     7     NA     NA      3     NA     NA      6
    ## 4     6     2     7     6     2     5     NA     NA      3     NA     NA     NA
    ## 5     6     5     6     4     5     6     NA     NA     NA      4     NA     NA
    ## 6     7     3     7     7     3     7     NA      2     NA     NA     NA     NA
    ##   Q193_T_VERB Q194 Q195 Q196 Q197 Q198 Q199 Q200 Q201_1 Q201_2 Q201_3 Q201_4
    ## 1                6 1951    1    1    1    3    2      7      7      7      7
    ## 2                3 1978    1    1    1    3    2      7      7      7      7
    ## 3                2 1991    1    1    1    3    2      7      7      7      7
    ## 4                1 1998    1    1    1    1    2      7      7      7      7
    ## 5                6 1986    1    1    1    3    2      7      7      7      7
    ## 6                2 1952    1    1    1    3    1     NA     NA     NA      4
    ##   Q202_1 Q202_2 Q202_3 Q202_4 Q202_5 Q202_6 Q202_7 Q202_8 Q202_9 Q202_10
    ## 1     NA      2     NA     NA     NA     NA     NA     NA     NA      NA
    ## 2     NA      2     NA     NA     NA     NA      7     NA     NA      NA
    ## 3     NA      2      3     NA     NA     NA      7     NA     NA      NA
    ## 4     NA     NA     NA     NA      5     NA      7     NA     NA      NA
    ## 5      1     NA     NA     NA     NA     NA     NA     NA     NA      NA
    ## 6     NA     NA     NA     NA     NA     NA     NA      8     NA      NA
    ##   Q202_VERB Q203_1 Q203_2 Q203_3 Q204 Q205 Q205_I Q206 Q207 Q208 Q209_1 Q209_2
    ## 1               NA     NA     NA   NA    2      2   NA    2    1      1     NA
    ## 2               NA     NA     NA   NA    2      2   NA    2    2     NA      2
    ## 3               NA     NA     NA   NA    2      2   NA    2    2     NA      2
    ## 4               NA     NA     NA   NA    4      4   NA    2    3     NA     NA
    ## 5               NA     NA     NA   NA    1      1   NA    1    2     NA      2
    ## 6               NA     NA     NA   NA    1      1   NA    1    1      1     NA
    ##   Q209_3 Q209_4 Q209_5 Q209_6 Q209_7 Q209_8 Q209_9 Q209_10 Q209_11 Q209_12 Q210
    ## 1     NA     NA     NA     NA     NA     NA     NA      NA      NA      NA    1
    ## 2     NA     NA     NA     NA     NA     NA     NA      NA      NA      NA    1
    ## 3     NA     NA     NA     NA     NA     NA     NA      NA      NA      NA    3
    ## 4     NA      4     NA     NA     NA     NA     NA      NA      NA      NA    1
    ## 5     NA     NA     NA     NA     NA     NA     NA      NA      NA      NA    1
    ## 6     NA     NA     NA     NA     NA     NA     NA      NA      NA      NA    1
    ##   Q211 Q212 Q213 Q214 Q215 Q216 Q217 Q218 Q219_1 Q219_2 Q219_3 Q219_4 Q219_5
    ## 1   10    2    6    1    7    7    7    2      7      7      7      7      7
    ## 2   10   13    6    1    7    7    7    2      7      7      7      7      7
    ## 3   13    1    6    4    1    2    2    2      7      7      7      7      7
    ## 4   12    1    6    1    7    7    7    2      7      7      7      7      7
    ## 5    1    1    6    4    1    2    1    2      7      7      7      7      7
    ## 6   11    1    6    1    7    7    7    2      7      7      7      7      7
    ##   Q220 Q221_1 Q221_2 Q221_3 Q222 Q223 Q224 Q225 Q226 Q227 Q228 T1Q27 T1Q28
    ## 1    2     NA     NA     NA    1    1    2    2    3    3    1     2    NA
    ## 2    2     NA     NA     NA    2    1    6    7    6    7    5     1    NA
    ## 3    2     NA     NA     NA    2    1    5    1    5    6    4     1    NA
    ## 4    2     NA     NA     NA    1    1    1    4    2    1    1     1    NA
    ## 5    2     NA     NA     NA    2    1    6    6    6    6    3     1    NA
    ## 6    2      1     NA     NA    2    1    6    6    6    7    7     1    NA
    ##   T1Q198 T1Q200 CIS_Q52 CIS_Q142 CIS_Q143_1 CIS_Q143_2 CIS_Q143_3 CIS_Q143_4
    ## 1      8      4      NA       NA         NA         NA         NA         NA
    ## 2      5      5      NA       NA         NA         NA         NA         NA
    ## 3      7      4      NA       NA         NA         NA         NA         NA
    ## 4      8      2      NA       NA         NA         NA         NA         NA
    ## 5      6      6      NA       NA         NA         NA         NA         NA
    ## 6      4      4      NA       NA         NA         NA         NA         NA
    ##   CIS_Q143_5 CIS_Q143_6 CIS_Q143_7 CIS_Q144 CIS_Q145 SOCIALWB SOCIALWB_I
    ## 1         NA         NA         NA       NA       NA 4.866667   4.866667
    ## 2         NA         NA         NA       NA       NA 4.266667   4.266667
    ## 3         NA         NA         NA       NA       NA 4.266667   4.266667
    ## 4         NA         NA         NA       NA       NA 3.200000   3.200000
    ## 5         NA         NA         NA       NA       NA 4.266667   4.266667
    ## 6         NA         NA         NA       NA       NA 5.600000   5.600000
    ##       MEIM   MEIM_I KESSLER6 KESSLER6_I AUDITC AUDITC_I DUDIT DUDIT_I EVERYDAY
    ## 1 2.000000 2.000000        1          1      6        6     6       6 2.111111
    ## 2 2.000000 2.000000        5          5      1        1     0       0 1.333333
    ## 3 2.666667 2.666667       11         11      6        6     4       4 3.111111
    ## 4 3.666667 3.666667       20         20      0        0     0       0 2.444444
    ## 5 4.833333 4.833333        8          8      0        0     0       0 2.777778
    ## 6 3.166667 3.166667        0          0      1        1     0       0 1.555556
    ##   EVERYDAY_I CHILDGNC CHILDGNC_I SOCSUPPORT SOCSUPPORT_I SOCSUPPORT_SO
    ## 1   2.111111        2          2   5.416667     5.416667          5.75
    ## 2   1.333333        1          1   7.000000     7.000000          7.00
    ## 3   3.111111       NA          2   5.250000     5.250000          7.00
    ## 4   2.444444        2          2   4.833333     4.833333          6.00
    ## 5   2.777778        2          2   5.750000     5.750000          6.25
    ## 6   1.555556        2          2   6.000000     6.000000          7.00
    ##   SOCSUPPORT_SO_I SOCSUPPORT_FAM SOCSUPPORT_FAM_I SOCSUPPORT_FR SOCSUPPORT_FR_I
    ## 1            5.75           4.50             4.50          6.00            6.00
    ## 2            7.00           7.00             7.00          7.00            7.00
    ## 3            7.00           1.75             1.75          7.00            7.00
    ## 4            6.00           2.25             2.25          6.25            6.25
    ## 5            6.25           6.00             6.00          5.00            5.00
    ## 6            7.00           4.00             4.00          7.00            7.00
    ##   LIFESAT LIFESAT_I CONNECTEDNESS CONNECTEDNESS_I HCTHREAT HCTHREAT_I
    ## 1     2.2       2.2           3.6             3.6     2.00       2.00
    ## 2     6.2       6.2           3.8             3.8     4.00       4.00
    ## 3     4.2       4.2           2.6             2.6     4.75       4.75
    ## 4     1.8       1.8           3.0             3.0     5.00       5.00
    ## 5     5.4       5.4           3.4             3.4     2.00       2.00
    ## 6     6.4       6.4           4.0             4.0     1.00       1.00
    ##   INTERNALIZED INTERNALIZED_I NONDISCLOSURE NONDISCLOSURE_I NONAFFIRM
    ## 1     2.166667       2.166667            NA             3.4  3.166667
    ## 2     2.833333       2.833333           3.0             3.0  1.166667
    ## 3     2.666667       2.666667           3.8             3.8  2.333333
    ## 4     4.000000       4.000000           3.6             3.6  5.000000
    ## 5           NA       1.833333           3.2             3.2  1.000000
    ## 6     2.166667       2.166667           2.0             2.0  1.166667
    ##   NONAFFIRM_I NEGEXPFUTURE NEGEXPFUTURE_I ACE ACE_I ACE_EMO ACE_PHY ACE_SEX
    ## 1    3.166667     2.555556       2.555556   1     1       1       0       0
    ## 2    1.166667     4.000000       4.000000   1     1       0       0       0
    ## 3    2.333333     5.000000       5.000000   1     1       0       0       0
    ## 4    5.000000     3.888889       3.888889  NA     6       1       0       0
    ## 5    1.000000           NA       2.666667   1     1       1       0       0
    ## 6    1.166667     1.222222       1.222222   1     1       0       0       1
    ##   ACE_IPV ACE_SUB ACE_MEN ACE_SEP ACE_INC ACE_EMO_I ACE_PHY_I ACE_SEX_I
    ## 1       0       0       0       0       0         1         0         0
    ## 2       0       0       1       0       0         0         0         0
    ## 3       0       0       1       0       0         0         0         0
    ## 4      NA       1       1       1       1         1         0         0
    ## 5       0       0       0       0       0         1         0         0
    ## 6       0       0       0       0       0         0         0         1
    ##   ACE_IPV_I ACE_SUB_I ACE_MEN_I ACE_SEP_I ACE_INC_I
    ## 1         0         0         0         0         0
    ## 2         0         0         1         0         0
    ## 3         0         0         1         0         0
    ## 4         1         1         1         1         1
    ## 5         0         0         0         0         0
    ## 6         0         0         0         0         0

### Data cleaning + wrangling

The process of preparing the data for analysis was fairly
straightforward because the TransPop data is already in tidy format. The
raw data is pretty large, with 1436 rows and 612 columns. Because the
scope of this particular research is pretty narrow, I decided to create
a smaller subset of the dataset for my analysis which only included TGNC
adults and the specific variables included in my analysis.

To do this, I used the `filter` function from the `dplyr` subpackage to
subset the data to only include TGNC adults. Then I used the `select`
function to choose only the columns I needed for my analysis in my
subset. Finally, I used the `mutate` function to designate gender
identity, race, and sexual minority identity as factors, which is
neccessary because otherwise R will treat these variables as numeric
when they are actually categorical.

``` r
transonly <- transpop |>
  filter(GENDER_IDENTITY > 2) |>
  select(
    STUDYID,
    WEIGHT_TRANSPOP,
    #demographics
    GENDER_IDENTITY,
    RACE_RECODE_CAT5,
    SEXMINID,
    AGE,
    #trans-specific variables
    INTERNALIZED_I,
    EVERYDAY_I,
    CHILDGNC_I,
    NONDISCLOSURE_I,
    NEGEXPFUTURE_I,
    SOCSUPPORT_I,
    CONNECTEDNESS_I,
    NONAFFIRM_I,
    HCTHREAT_I,
    #positive health outcomes
    LIFESAT,
    SOCIALWB
  ) |>
  mutate(
    GENDER_IDENTITY = as.factor(GENDER_IDENTITY),
    RACE_RECODE_CAT5 = as.factor(RACE_RECODE_CAT5),
    SEXMINID = as.factor(SEXMINID)
  )
```

``` r
head(transonly)
```

    ##     STUDYID WEIGHT_TRANSPOP GENDER_IDENTITY RACE_RECODE_CAT5 SEXMINID AGE
    ## 1 151768927       0.9861429               4                1        0  65
    ## 2 152357242       0.3796825               3                1        0  38
    ## 3 152444055       0.7053812               3                1        1  25
    ## 4 152525272       1.5953976               3                1        1  18
    ## 5 152894493       1.8704222               3                3        0  30
    ## 6 152925625       0.9546502               3                4        0  64
    ##   INTERNALIZED_I EVERYDAY_I CHILDGNC_I NONDISCLOSURE_I NEGEXPFUTURE_I
    ## 1       2.166667   2.111111          2             3.4       2.555556
    ## 2       2.833333   1.333333          1             3.0       4.000000
    ## 3       2.666667   3.111111          2             3.8       5.000000
    ## 4       4.000000   2.444444          2             3.6       3.888889
    ## 5       1.833333   2.777778          2             3.2       2.666667
    ## 6       2.166667   1.555556          2             2.0       1.222222
    ##   SOCSUPPORT_I CONNECTEDNESS_I NONAFFIRM_I HCTHREAT_I LIFESAT SOCIALWB
    ## 1     5.416667             3.6    3.166667       2.00     2.2 4.866667
    ## 2     7.000000             3.8    1.166667       4.00     6.2 4.266667
    ## 3     5.250000             2.6    2.333333       4.75     4.2 4.266667
    ## 4     4.833333             3.0    5.000000       5.00     1.8 3.200000
    ## 5     5.750000             3.4    1.000000       2.00     5.4 4.266667
    ## 6     6.000000             4.0    1.166667       1.00     6.4 5.600000

This subset now has 274 rows and 17 columns, which will be much more
efficient for fitting models.

------------------------------------------------------------------------

## Model fitting

The **partykit::ctree** package implements [conditional inference
trees](https://www.zeileis.org/papers/Hothorn+Hornik+Zeileis-2006.pdf),
a non-parametric regression tree method that embeds recursive
partitioning into a framework of conditional hypothesis testing (Hothorn
et al., 2006). For a more indepth explanation of this method, please see
the [package
documentation](https://cran.r-project.org/web/packages/partykit/vignettes/ctree.pdf).

Key features include:

1.  **Unbiased Splitting**:
    - Uses permutation tests to select covariates and split points,
      avoiding bias toward variables with many splits.  
    - Tests global independence (via *p*-values) between covariates and
      response; stops if no significant association exists.
2.  **Flexibility**:
    - Handles diverse response types (numeric, ordinal, censored,
      multivariate) and covariates (nominal, ordinal, numeric).  
    - Supports weights, missing data (via surrogate splits), and
      clustered observations.
3.  **Algorithm**:
    - **Step 1**: Select the covariate with the strongest association
      (smallest *p*-value).  
    - **Step 2**: Choose the optimal split by maximizing a standardized
      statistic (e.g., mean differences).  
    - Recursively repeats until stopping criteria (e.g., alpha-level,
      sample size) are met.

As stated previously, 13 variables were included as potential predictors
in the conditional inference tree models. Four total models were fitted;
one tree with only the 4 demographic variables included as predictors
for both outcome measures, and one tree with the 9 remaining variables
included as predictors for both outcome measures. The code for fitting
these models is included below.

``` r
# Life satisfaction - demographics only
lifesat_tree1 = ctree(
  formula = as.ordered(LIFESAT) ~ AGE + GENDER_IDENTITY + RACE_RECODE_CAT5 +
    SEXMINID,
  data = transonly,
  minbucket = 26,
  multiway = T,
  na.action = na.omit,
  weights = WEIGHT_TRANSPOP
)

# Social wellbeing - demographics only
socialwb_tree1 = ctree(
  formula = as.ordered(SOCIALWB) ~ AGE + GENDER_IDENTITY + RACE_RECODE_CAT5 +
    SEXMINID,
  data = transonly,
  minbucket = 26,
  multiway = T,
  na.action = na.omit,
  weights = WEIGHT_TRANSPOP
)

# Life satisfaction - stressors only
lifesat_tree_stressors = ctree(
  formula = as.ordered(LIFESAT) ~ INTERNALIZED_I +
    EVERYDAY_I + CHILDGNC_I + NONDISCLOSURE_I +
    NEGEXPFUTURE_I + SOCSUPPORT_I + CONNECTEDNESS_I +
    NONAFFIRM_I + HCTHREAT_I + SEXMINID,
  data = transonly,
  minbucket = 26,
  multiway = T,
  na.action = na.omit,
  weights = WEIGHT_TRANSPOP
)

# social wellbeing - stressors only
socialwb_tree_stressors = ctree(
  formula = as.ordered(SOCIALWB) ~ INTERNALIZED_I +
    EVERYDAY_I + CHILDGNC_I + NONDISCLOSURE_I +
    NEGEXPFUTURE_I + SOCSUPPORT_I + CONNECTEDNESS_I +
    NONAFFIRM_I + HCTHREAT_I + SEXMINID,
  data = transonly,
  minbucket = 26,
  multiway = T,
  na.action = na.omit,
  weights = WEIGHT_TRANSPOP
)
```

------------------------------------------------------------------------

## **Results**

#### **1. Demographic Predictors**

**Life Satisfaction (`LIFESAT`)**  
The model identified sexual minority status (`SEXMINID`) as the primary
splitting variable. Heterosexual participants (`SEXMINID`=0) exhibited
higher mean life satisfaction (*M* = 4.80) than sexual-minority
(`SEXMINID`=1) participants.

Among sexual minorities, `AGE` produced a secondary split. For those 23
years old or youger, race further differentiated outcomes: - White
participants (`RACE_RECODE_CAT5`=1) showed slighly lower life
satisfaction (*M* = 2) - Black, Latino/Hispanic, Multirace, or Other
(`RACE_RECODE_CAT5`=2-5) showed slightly higher life satisfaction
(*M*=2.20)

Participants older than 23 years had a mean life satisfaction score of
2.2 regardless of race.

    ## 
    ## Model formula:
    ## as.ordered(LIFESAT) ~ AGE + GENDER_IDENTITY + RACE_RECODE_CAT5 + 
    ##     SEXMINID
    ## 
    ## Fitted party:
    ## [1] root
    ## |   [2] SEXMINID in 0: 4.8000001907349 (w = 47.3, err = 84.4%)
    ## |   [3] SEXMINID in 1
    ## |   |   [4] AGE <= 23
    ## |   |   |   [5] RACE_RECODE_CAT5 in 1: 2 (w = 46.7, err = 85.1%)
    ## |   |   |   [6] RACE_RECODE_CAT5 in 2, 3, 4, 5: 2.2000000476837 (w = 33.0, err = 82.4%)
    ## |   |   [7] AGE > 23: 2.2000000476837 (w = 139.9, err = 92.3%)
    ## 
    ## Number of inner nodes:    3
    ## Number of terminal nodes: 4

![](transpop1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

**Social Well-Being (`SOCIALWB`)**  
No significant splits emerged, with the model yielding only a root node
(*M* = 5.13), indicating that demographic variables alone did not
meaningfully partition social well-being in this sample.

    ## 
    ## Model formula:
    ## as.ordered(SOCIALWB) ~ AGE + GENDER_IDENTITY + RACE_RECODE_CAT5 + 
    ##     SEXMINID
    ## 
    ## Fitted party:
    ## [1] root: 5.1333332061768 (w = 263.3, err = 93.2%) 
    ## 
    ## Number of inner nodes:    0
    ## Number of terminal nodes: 1

![](transpop1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

#### **2. Psychosocial and Stress-Related Predictors**

**Life Satisfaction (`LIFESAT`)**  
Everyday discrimination (`EVERYDAY_I`) was the primary partitioning
variable. Participants reporting lower discrimination (≤1.44) exhibited
the highest life satisfaction (*M* = 5.80). Among those reporting higher
discrimination (\>1.44), negative future expectations (`NEGEXPFUTURE_I`)
further differentiated outcomes. Those with fewer negative expectations
of the future for TGNC individuals (≤3.33) reported the greatest mean
(though still moderate) life satisfaction of this group (*M* = 4.80),
whereas those with greater negative expectations (\>3.33) were further
differentiated based on social support (`SOCSUPPORT_I`). Low support
(≤4.83) was associated with the lowest satisfaction (*M* = 1.00), while
higher support (\>4.83) corresponded to moderately better outcomes (*M*
= 2.20).

    ## 
    ## Model formula:
    ## as.ordered(LIFESAT) ~ INTERNALIZED_I + EVERYDAY_I + CHILDGNC_I + 
    ##     NONDISCLOSURE_I + NEGEXPFUTURE_I + SOCSUPPORT_I + CONNECTEDNESS_I + 
    ##     NONAFFIRM_I + HCTHREAT_I + SEXMINID
    ## 
    ## Fitted party:
    ## [1] root
    ## |   [2] EVERYDAY_I <= 1.44444: 5.8000001907349 (w = 56.1, err = 85.5%)
    ## |   [3] EVERYDAY_I > 1.44444
    ## |   |   [4] NEGEXPFUTURE_I <= 3.33333: 4.8000001907349 (w = 104.1, err = 88.3%)
    ## |   |   [5] NEGEXPFUTURE_I > 3.33333
    ## |   |   |   [6] SOCSUPPORT_I <= 4.83333: 1 (w = 47.9, err = 80.9%)
    ## |   |   |   [7] SOCSUPPORT_I > 4.83333: 2.2000000476837 (w = 58.8, err = 81.4%)
    ## 
    ## Number of inner nodes:    3
    ## Number of terminal nodes: 4

![](transpop1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

**Social Well-Being (`SOCIALWB`)**  
Social support (`SOCSUPPORT_I`) served as the initial splitting
criterion. Participants with high support (\>5.58) reported greater
well-being (*M* = 5.13). Among those with lower support (≤5.58),
subsequent splits occurred based on community connectedness
(`CONNECTEDNESS_I`) as a secondary factor and healthcare stereotype
threat (`HCTHREAT_I`) and non-affirmation of gender identity
(`NONAFFIRM_I`) as tertiary factors. The lowest well-being (*M* = 3.27)
was observed among individuals with low connectedness and high
healthcare threat, whereas the highest well-being in this subgroup (*M*
= 5.27) was found among those with high connectedness and fewer
experiences of non-affirmation.

    ## 
    ## Model formula:
    ## as.ordered(SOCIALWB) ~ INTERNALIZED_I + EVERYDAY_I + CHILDGNC_I + 
    ##     NONDISCLOSURE_I + NEGEXPFUTURE_I + SOCSUPPORT_I + CONNECTEDNESS_I + 
    ##     NONAFFIRM_I + HCTHREAT_I + SEXMINID
    ## 
    ## Fitted party:
    ## [1] root
    ## |   [2] SOCSUPPORT_I <= 5.58333
    ## |   |   [3] CONNECTEDNESS_I <= 3.4
    ## |   |   |   [4] HCTHREAT_I <= 3.25: 4 (w = 33.4, err = 84.4%)
    ## |   |   |   [5] HCTHREAT_I > 3.25: 3.2666666507721 (w = 59.5, err = 86.2%)
    ## |   |   [6] CONNECTEDNESS_I > 3.4
    ## |   |   |   [7] NONAFFIRM_I <= 3.16667: 5.2666668891907 (w = 47.9, err = 76.6%)
    ## |   |   |   [8] NONAFFIRM_I > 3.16667: 4.1999998092651 (w = 37.2, err = 88.6%)
    ## |   [9] SOCSUPPORT_I > 5.58333: 5.1333332061768 (w = 85.3, err = 87.2%)
    ## 
    ## Number of inner nodes:    4
    ## Number of terminal nodes: 5

![](transpop1_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

------------------------------------------------------------------------

### **Discussion**

The conditional inference trees revealed distinct patterns in how
demographic and psychosocial variables partitioned life satisfaction and
social well-being.  

These findings align with prior research linking minority stress and
social support to well-being in TGNC populations. The prominence of
discrimination and support in the trees underscores their relevance in
models of TGNC health. Future studies using longitudinal or experimental
designs may help clarify the temporal and directional dynamics of these
relationships. A significant limitation of the present study is that
only a few key factors were considered, and a mediation analysis with
further moderating factors was not performed. Future studies should
consider including more factors and a mediation analysis. Another
limitation of this paper is that, bluntly, I could not figure out how to
make the plots look better which leads to slightly confusing
visualization.  

Trans lives deserved to be cherished, honored, and protected. Beyond
that TGNC deserve to flourish. These findings highlight the importance
of creating and fostering safe spaces for TGNC individuals to connect
with (`CONNECTDEDNESS_I`) and support (`SOCIAL_SUPPORT`) one another,
especially for TGNC youth. Additionally, increasing awareness of the
adverse effects of invalidating identities (`NONAFFIRM_I`) and creating
trans-inclusive environments within healthcare (`HCTHREAT_I`) could help
TGNC induviduals thrive. Fostering community and hope for the future
(`NEGEXPECT_I`) within these supportive spaces might also increase how
satisfied TNGNC individuals are with their lives. Of course, reducing
instances of everyday discrimination would be ideal, and potentially
could increase the life satisfaction of TGNC adults as demonstrated by
the model (`EVERYDAY_I`).  

**Though this may feel like too big of a hurdle, transgender activists
such as Marsha P Johnson and Sylvia Rivera made massive strides in how
TGNC folks are treated and viewed in society today. The torch is now
ours, and it’s important not to give up hope. Trans is forever.**

> If transgender rights are important to you, please let your
> representatives know by signing [this petition from the
> ACLU](https://action.aclu.org/petition/defend-trans-freedom).

> To learn more about the ACLU’s role in the upcoming supreme court case
> and the stories of TGNC individuals impacted by it, see the [“Freedom
> to Be”
> Campaign](https://www.aclu.org/campaigns-initiatives/freedom-to-be).

------------------------------------------------------------------------

## Local resources for TGNC folks

The **Massachusetts Transgender Political Coalition** offers **free
gender affirming garments** through their **G.E.A.R.** – *Gender
Euphoria and Affirmation Resources* initiative. To learn more and/or
request garments, visit their website
[here](https://www.masstpc.org/what-we-do/gear/).

Simmons University is steps away from **Fenway Health**, a national
leader in gender affirming healthcare and research and actually was a
primary contributor to the TransPop study. **They are currently
accepting new patients**. Learn more about their services and/or request
a provider visit their website
[here](https://fenwayhealth.org/care/medical/transgender-health/).

------------------------------------------------------------------------

[<img src="sweatermuppet_dontdiewondering.jpg" width="300"
alt="Original artwork from Silas Denver" />](https://ko-fi.com/i/IH2H31829O7)

[<img src="marsha_rivera.jpeg" width="300"
alt="Marsha P Johnson, Sylvia Rivera and other activists outside of New York City Hall in 1973" />](https://digitalcollections.nypl.org/items/510d47e3-57a1-a3d9-e040-e00a18064a99)

##### To read about the roots of the transgender rights movement and the Street Transvestite Action Revolutionaries organization spearheaded by Marsha P Johnson & Sylvia Rivera, please see this [edited collection from Untorelli Press](https://web.archive.org/web/20190309213709/https://untorellipress.noblogs.org/files/2011/12/STAR.pdf).

## References

Hothorn T, Hornik K, Zeileis A (2006). “Unbiased Recursive Partitioning:
A Conditional Inference Framework.” Journal of Computational and
Graphical Statistics, 15(3), 651–674.
<a href="https://doi:10.1198/106186006X133933"
class="uri">https://doi:10.1198/106186006X133933</a>

Hothorn T, Zeileis A (2015). “partykit: A Modular Toolkit for Recursive
Partytioning in R.” Journal of Machine Learning Research, 16, 3905-3909.
<https://jmlr.org/papers/v16/hothorn15a.html>.

Krueger, E. A., Divsalar, S., Luhur, W., Choi, S. K., & Meyer, I. H.
(2020). TransPop - U.S. Transgender Population Health Survey
(Methodology and Technical Notes). Los Angeles, CA: The Williams
Institute. Retrieved from:
<https://www.transpop.org/s/TransPop-Survey-Methods-v18-FINAL-copy.pdf>

Meyer, I. H. (2021). TransPop, United States, 2016-2018 \[Dataset\].
Inter-university Consortium for Political and Social Research
\[distributor\]. <https://doi.org/10.3886/ICPSR37938.v1>
