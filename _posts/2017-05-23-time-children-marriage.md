---
layout: "post"
title: "Is it time? Marriage and Births in data"
date: 2017-05-23
categories: jekyll update
image: mother_firstchild.png
---
Looking at my Facebook newsfeed today is very different from what it used to be 5 years ago. At that time the newsfeed was filled with the typical posts, which can be categorised like this: holidays, parties, food and the occasional dog/cat. Now something has changed, the list grew by two more items... weddings and babies. 
This is when I realised that I must have reached the critical age now. But when is actually the time that people get their first child? 

For this, as so often, I browsed the eurostats database. The average age of mothers at childbirth is about 30 years (+/- 1.5 years) and that is very similar across the EU countries. However, this is averaged over all births, counting in the second, third and so on child. Looking at the mother's age at the birth of the **first** child there was a bit more variety between the countries. 
The first recorded data in this set is from 2006, which I included in the analysis.

{% highlight R %}
ggplot(data = m, aes(x=GEO, y=Value, color=TIME)) + 
  geom_point(size=4) +   # Draw points
  scale_color_manual(values = c("grey", "darkblue"))+
  geom_segment(aes(x=GEO, 
                   xend=GEO, 
                   y=min(Value), 
                   yend=max(Value)), 
               linetype="dashed", 
               size=0.1) +   # Draw dashed lines
  labs(title="Age of mother at birth of first child") +  
  coord_flip()+
  theme_bw()+
  scale_y_continuous(breaks=seq(23,31,1))+
  theme(line=element_line(size=1, color="black"),
        text = element_text(size=10, face="bold"),
        panel.border = element_rect(size = 1, colour = "black"),
        axis.title.x = element_blank(),
        axis.title.y = element_blank())

{% endhighlight %}
![mother_firstchild](/assets/birthrate/mother_firstchild.png){:class="img-responsive"}

I was surprised to see such a clear change in the last few years. What is clear from this dot plot is that in the last 9 years, on average, mothers are about 1-2 years older when their first child is born. Only two countries do not fall within this pattern, UK and Azerbaijan. Usually not two countries which are named together. 

The eurostats data set contained also another interesting record - the proportion of children born outside marriage. Quickly browsing through the data showed a great discrepancy from about 2% in Turkey to over 60% in Iceland. 
Could religion explain the differences in the ratio of babys born outside marriage between these countries?

To address this, I first had to find how to measure religion in a country?  Browsing the internet I found out that the European Commision carries out standardised surveys within Europe since 1974. According to their homepage, each survey consits of about 1000 face-to-face interviews per country. On a wikipedia page I found a survey which suited very well. "Do you believe in God", was the question measured across the european countries which I used for this analysis. The survey was carried out in 2010, which I matched to the birth/marriage data.

It is fairly easy to integrate a table from a wikipedia page into R.
{% highlight R %}
library(rvest)
u<-"https://en.wikipedia.org/wiki/Religion_in_Europe"
religion <- u %>%
  read_html() %>%
  html_nodes(xpath='//*[@id="mw-content-text"]/table[1]') %>%
  html_table()
{% endhighlight %}

After a little bit of tidying I could correlate the ratio of children born outside marriage with the percentage of people who believe in God, for each country.

{% highlight R %}
library(ggplot2)
ggplot(data = god_marriage, aes(x=ratioBirthsUnmarried ,y=God))+
  geom_point()+
  geom_text(aes(label=Country), hjust=-0.1, vjust=0)+
  geom_smooth(method = lm)+
  geom_hline(yintercept = 50, color = "red", linetype = "dashed")+
  geom_vline(xintercept = 50, color = "red", linetype = "dashed")+
  labs(title="Rate of children born to unmarried parents", 
       x="% children born to unmarried parents",
       y="% people believing in God") +  
  theme_bw()+
  theme(line=element_line(size=1, color="black"),
        text = element_text(size=15, face="bold"),
        panel.border = element_rect(size = 1, colour = "black"))

{% endhighlight %}
![birthrate_god](/assets/birthrate/cor_birthrate_god.png){:class="img-responsive"}

The dashed red-cross shows the 50/50 split which helps to orientate yourself on the graph. In most countries, the majority of children are still born to married couples, unlike the countries in the bottom-right corner. We can see a very strong negative correlation. I try not to use the word "significant" without having tested for it, so I performed the stats. 

{% highlight R %}
cor(x=god_marriage$God, y=god_marriage$ratioBirthsUnmarried)
linearMod <- lm(God ~ ratioBirthsUnmarried, data=god_marriage)
summary(linearMod)
{% endhighlight %}

There is a strong negative correlation of -0.8 between these two parameters, which are **significantly** associated with each other. More,from the R-sqaured we see that we can explain about 65% of variation between the countries with the fact, with the percentage of how many people believe in God in this country.

Overall, it is surprising to see that in almost all countries couples wait longer to get their first child. Also, we can see that there is a strong relationship between believing in God and getting married. This fact explains most of the differences in the ratio of children born to married and unmarried couples across European countries. 

Finally, I can also confirm that searching the internet for baby data and thanks to personalised advertisment, it is not only my Facebook feed that is full of babies and baby products.