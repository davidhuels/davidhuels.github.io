---
layout: post
title: "What if... The UK election 2017"
date: 2017-06-15
categories: jekyll update
image: polling_day_preview.jpg
---

The General Election last week had a clear winner. Opposing the politics of Theresa May, and mobilising many young voters (at least according to his twitter account), it was a night to remember. 
Stephen Gethins (SNP), MP for Fife North East, won his seat by two votes, defeating Elizabeth Riches with [13743 vs. 13741 votes][bbc]. Truly, every vote matters.

After seeing this result, I was wondering if there were more such close wins. What would have happened if these narrow wins would have gone the other way? 

Since it takes quite a while until the Electoral Office releases the data in a nicely ordered file, I downloaded the results from the BBC websites. Looping through all 650 constituencies I gathered the information I needed.

First I analysed how the Conservatives and the Labour won their constituencies. In a histogram I plotted how these two parties won their respective constituencies. 

![histogram_wins](/assets/GE2017/histo_win_percent.png){:class="img-responsive", alt="Histogram_LAB_CON_wins"}

The conservatives won their seats with an average of 55% of the votes, whereas Labour won their constituencies with 59% of the votes. Interestingly, we can also see that Labour has a much broader distribution, reaching much further to the right. Here, we can find the Labour safe seats, where 70% and more voters elected a Labour MP, e.g. Birmingham, London, Liverpool and Manchester.

![Labour_strongholds](/assets/GE2017/uk_ge2017_labour_highWins.png){:class="img-responsive", alt="map_labour_strongholds"}

Back, to the original question, what would have happened if, let's say any constituencies which were won by a margin of 5% or less, would have gone to the second "winner". A total of 97/650 seats were decided by a margin of 5% or less. Let's see how it looks if we swap the winners...

**Party**|**seats**|**seats after 5% margin swap**
:-----:|:-----:|:-----:
CONConservative|317|310
DUPDemocratic Unionist Party|10|9
GRNGreen Party|1|1
LABLabour|262|277
LDLiberal Democrat|12|14
PCPlaid Cymru|4|2
SFSinn Fein|7|5
SNPScottish National Party|35|27


![barplot_seats](/assets/GE2017/ge2017_seats.png){:class="img-responsive", alt="Barplot_seats_before and after_5%swap"}

The attentive reader might spot that I actually only have 317 seats assigned to the Conservatives whereas they officially won 318 seats. Responsible for this is John Bercow, the Speaker of the House of Commons. Although member of the Conservative Party, taking office he renounces all affiliations with his former party, which means in my file he is assigned as Speaker and not as a Conservative.

![map_results_GE2017](/assets/GE2017/uk_ge2017_results.png){:class="img-responsive", alt="map_GE2017_results"}

Overall, this analysis showed that the General Election was not a close race and even if the closely won/lost constituencies would have changed, the outcome of this Election would have stayed the same. However, a coalition of the Conservatives with the DUP would not be sufficient. So, I guess although it doesn't feel like a win for Theresa May, it could have gone **a bit** worse.




[bbc]: http://www.bbc.com/news/uk-scotland-scotland-politics-40214545