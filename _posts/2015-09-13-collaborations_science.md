---
layout: post
title:  Collaborations in Science across the World
date:   2015-09-13
categories: jekyll update
---
In one of my earlier posts I showed that over the last 20 years the increase in the number of authors can be largely attributed to the genomic sequencing efforts. In this post I took a closer look at the number of authors per journal and tried to quantify collaborations, national and international.


In the last few months, articles were published which went far beyond the usual number of authors for a single publication, especially in biology. A [fruit-fly paper][fruit_fly] had more than 1,000 authors, of whom 900 were graduate students. In physics it is more common to have a large number of authors. A [paper][lhc] from the Large Hadron Collider recently set the record with 5,154 authors.

As a change to my earlier posts, I separated the code from the descriptive part. I will describe the analysisof the data in a later post. The original .csv file and the function how to obtain the data from pubmed can be found on github.

Briefly, I wrote an R-script, based on the XML package that would extract from each research article for a given journal the number of authors, the affiliations and the countries from each affiliation. This resulted for the year 2014 in a total of 20 625 articles. Before 2014 pubmed only contained the affiliation of the first author, but they changed in 2014 and now contain the affiliations of all the authors.

I focused the search only on research articles (as defined by the presence of an abstract in pubmed) in the year 2014 across a range of 24 journals. These journals were chosen randomly, but the idea was to cover low- and high-impact journals.

![Authors_Median](/assets/collaborations_science/authors_median_if1.png){:class="img-responsive"}

Again, the number of authors is not normally distributed and is skewed to the right. Therefore I used the median to describe the most frequent number of authors. We can see that there is a significant positive correlation ( cor=0.65, p=0.0006) between the median author number and the impact factor (IF) of each journal. The two outliers, Nature Genetics and the New England Journal of Medicine have an even higher number of authors. The average number of authors for the New England Journal of Medicine is actually more than 109, which is the reason why most of N Eng J Med entries in pubmed have not listed the affiliations. Therfore I removed N Eng J Med for the following analyses.

I then organised the articles by the country of the first author (country of origin). So which country had the most articles published in 2014?

![publications_absolute](/assets/collaborations_science/publications_absolute.png){:class="img-responsive"}

By far the most articles came from the United States, followed by China. But when I analyse the number of articles for each country relative to their population, the picture changed.

![publications_capita](/assets/collaborations_science/publications_capita.png){:class="img-responsive"}

Now, Switzerland is taking the lead, followed by Sweden and Denmark. The United States are found on the lower ranks and China didn’t even make it to the top 20. I am not sure if the Dominica entry is true, but since this little island in the Carribean has only a population of 72 000, they only need 1-2 papers.

Looking at the individual countries further I wanted to see which country is the most collaborative. I used the number of unique affiliations for each article as the number of collaborations. I only included countries which had more than 30 articles published in these journals in 2014.


| Origin.Country | Publications | Authors.mean | Authors.median | Affiliations.mean |Countries per publ. | 
|----------------|--------------|--------------|----------------|-------------------|---------------------------| 
| Finland        | 141          | 12.91        | 7              | 5.15              | 1.99                      | 
| United Kingdom | 1590         | 13.65        | 7              | 5.12              | 2.02                      | 
| Denmark        | 205          | 10.86        | 6              | 4.97              | 1.91                      | 
| Netherlands    | 498          | 9.76         | 7              | 4.87              | 1.93                      | 
| France         | 853          | 11.36        | 8              | 4.81              | 1.90                      | 
| South Africa   | 77           | 10.81        | 6              | 4.79              | 2.35                      | 
| Spain          | 522          | 10.22        | 7              | 4.76              | 1.81                      | 
| Taiwan         | 285          | 7.49         | 7              | 4.56              | 1.32                      | 
| Belgium        | 238          | 15.83        | 8              | 4.50              | 2.43                      | 
| United States  | 5508         | 10.37        | 6              | 4.42              | 1.55                      | 
| Australia      | 618          | 9.74         | 6              | 4.28              | 1.81                      | 
| Singapore      | 124          | 9.09         | 7              | 4.27              | 1.88                      | 
| Germany        | 1381         | 9.46         | 7              | 4.23              | 1.72                      | 
| Canada         | 812          | 9.56         | 6              | 4.19              | 1.71                      | 
| Greece         | 38           | 6.84         | 6              | 4.18              | 2.08                      | 
| Switzerland    | 403          | 9.78         | 7              | 4.16              | 2.02                      | 
| Italy          | 533          | 13.38        | 8              | 4.15              | 1.63                      | 
| Portugal       | 97           | 7.61         | 7              | 4.11              | 2.02                      | 
| Korea          | 380          | 8.70         | 8              | 4.09              | 1.44                      | 
| Norway         | 128          | 7.77         | 6              | 4.05              | 1.73                      | 
| Japan          | 954          | 9.54         | 8              | 3.92              | 1.25                      | 
| Brazil         | 309          | 8.27         | 7              | 3.89              | 1.60                      | 
| Austria        | 188          | 8.90         | 8              | 3.82              | 1.77                      | 
| Israel         | 240          | 10.53        | 6              | 3.78              | 1.73                      | 
| Ireland        | 62           | 14.00        | 6              | 3.76              | 2.03                      | 
| Hungary        | 51           | 8.35         | 8              | 3.71              | 1.59                      | 
| Thailand       | 53           | 6.58         | 6              | 3.70              | 1.75                      | 
| Sweden         | 330          | 18.98        | 6              | 3.67              | 1.78                      | 
| Georgia        | 132          | 7.14         | 6              | 3.57              | 1.81                      | 
| China          | 2566         | 8.87         | 7              | 3.48              | 1.37                      | 
| Mexico         | 61           | 7.59         | 6              | 3.48              | 1.66                      | 
| Poland         | 85           | 6.62         | 6              | 3.39              | 1.48                      | 
| Argentina      | 58           | 21.52        | 7.5            | 3.38              | 1.81                      | 
| Czech Republic | 67           | 9.04         | 6              | 3.33              | 1.66                      | 
| Chile          | 39           | 7.49         | 6              | 3.23              | 1.69                      | 
| Malaysia       | 55           | 5.56         | 5              | 3.16              | 1.44                      | 
| New Zealand    | 50           | 5.82         | 5              | 3.02              | 1.74                      | 
| India          | 341          | 8.67         | 6              | 2.99              | 1.38                      | 
| Russia         | 46           | 6.09         | 5              | 2.83              | 1.59                      | 


The number of collaborations are almost evenly distributed between 2.5 and 5. Only Finland and the UK have on average more than 5 collaborations per paper.

![affiliations_histogram](/assets/collaborations_science/affiliations_histogram.png){:class="img-responsive"}

I then looked at the different countries in the affiliations for each article to get an idea how many international collaborations were involved.

Plotting the international collaborations revealed a beautiful normal distribution with a mean of 1.7 (sd=0.258) international collaborations across the countries. Interestingly, Belgium was the country with the most international collaborations (followed by South Africa).

![international_collaborations](/assets/collaborations_science/international_collaborations.png){:class="img-responsive"}

Looking at the correlation of the total collaborations (affiliations) versus the international collaborations we can see some interesting preferences, which do not fit the linear correlation.

It seems Belgium has a clear preference for international collaborations, whereas Japan and Taiwan rather collaborate within their country.

![affiliations_int_coll](/assets/collaborations_science/affiliations_intcoll_correlation.png){:class="img-responsive"}

The last bit I wanted to analyse, what is the favourite country to collaborate with? Here, every country on the map is coloured by their most frequent country for a collaboration. We can see that most countries have the United States as their favourite collaboration across the borders.

![top_coll](/assets/collaborations_science/top_collaboration_across_world.png){:class="img-responsive"}

In summary we can say that the higher the impact factor the more authors are involved. In total number of publications, the United States are still dominating the world of science, but reduced to the population, Switzerland and Europe aren’t doing bad either. The most collaborative countries are Finland and the UK. International collaborations are pretty similar all over the world, but Belgium for example has a clear preference to reach across the border. Still, the most popular country for a collaboration are the United States.

[fruit_fly]:http://www.nature.com/news/fruit-fly-paper-has-1-000-authors-1.17555
[lhc]:http://www.nature.com/news/physics-paper-sets-record-with-more-than-5-000-authors-1.17567