---
layout: post
title:  Friends don’t let friends use two y-axes
date:   2016-12-30
categories: jekyll update
image: economist.png
---
I recently came across an article in the economist in which the author describes a correlation between joblessness and the rise in wages. The authors say that”Economists expect that when joblessness falls, wages will rise…”. However, after the recession, the wages are not going up as predicted. I usually don’t give much notice to economical trends, but here I was curious as the graph was shown with two y-axes.

For copyright reasons I can’t display the graph here, but it can be found under this [link][economist]. Although using two y-axes seems intuitive it is not well appreciated. There is an excellent post by [Kieran Healey][kieran] explaining the pitfalls of such graphs with the good rule of “Friends don’t let friends use two y-axes”. Not without reason are the weirdest correlations often presented with two y-axes. My favourite is the correlation between the age of Miss America and murders by steam, hot vapours and hot objects [(spurious correlations)][spurious].  So I thought I try to recreate the graphs in R. Jeffrey Arnold created a great package for ggplot2 (ggthemes) which allows you to customise your graphs so that they copy the look of certain newspapers (economist, FT…). They just look more professional and even the Miss America correlation would look more serious.

![economist_picture1](/assets/two_yaxes/economist_change_absoloute_red.png){:class="img-responsive"}


In the economist article the graphs started in Jan 2005 but we are going to use the full data set. As we see, the financial crisis 2008-2009 had a major impact on both the unemployment rate and the changes in average earnings. However, looking at the time before 2006, we observe that the unemployment rate was quite stable whereas the change in average earnings shows a big dip.

Although both graphs have % units on the y-axes, they don’t mean the same thing. Whereas the first graph shows changes in the rate of the wage in percent, the second is the percentage of unemployed people in Britain, an absolute number. If we use only the absolute numbers in both cases, we get a very different picture. True, this is not what the economist wanted to show, but it is important to keep in mind that the change in the growth-rate should not be confused with the actual average earning.

![economist_picture2](/assets/two_yaxes/economist_absoluteNo_red.png){:class="img-responsive"}


So is there actually a correlation between these two? Let’s have a look a the scatterplot.

![economist_picture3](/assets/two_yaxes/economist_correlation_red.png){:class="img-responsive"}


There seems to be a negative correlation between the unemployment rate and the change in average earnings. Nevertheless, this correlation is based on data over a long timescale. So as a non-economist, I would suggest a little patience. If the earnings are going up in the near future, everything will be back to normal. If not, well at least Miss America 2015 was only 22, so we should expect less murders by steam, right?!

The ONS data can be found here for [unemployment] and [average earnings].

Here is the code for the graphs.

{% highlight R %}
ggplot(data=econ, aes(x=Date, y=value, colour=variable))+
geom_line(size=2)+
scale_color_manual(values = c("blue", "darkred"),
name="Britain",
labels=c("Average earnings % change",
"Unemployment rate %"))+
facet_grid(variable ~ ., scales = "free_y")+
expand_limits(y=0)+
theme_economist()+
theme(panel.margin = unit(1, "cm"),
panel.border = element_rect(fill = NA, colour = "black", size = 1),
axis.title.x = element_blank(),
axis.title.y = element_blank())
 
ggplot(data=econ3, aes(x=Date, y=value, colour=variable))+
geom_line(size=2)+
scale_color_manual(values = c("blue", "darkred"),
name="Britain",
labels=c("Weekly earnings [pounds]",
"Unemployment rate %"))+
facet_grid(variable ~ ., scales = "free_y")+
expand_limits(y=0)+
theme_economist()+
theme(panel.margin = unit(1, "cm"),
panel.border = element_rect(fill = NA, colour = "black", size = 1),
axis.title.x = element_blank(),
axis.title.y = element_blank())
 
econ_corr2<-econ_corr
econ_corr2$yearP<-as.integer(econ_corr2$year)
head(econ_corr2)
class(econ_corr2$yearP)
econ_corr2$yearP[econ_corr2$yearP<=2003]<-"2001-2003"
econ_corr2$yearP[econ_corr2$yearP>2003 & econ_corr2$yearP<=2006]<-"2003-2006"
econ_corr2$yearP[econ_corr2$yearP>2006 & econ_corr2$yearP<=2009]<-"2006-2009"
econ_corr2$yearP[econ_corr2$yearP>2009 & econ_corr2$yearP<=2012]<-"2009-2012"
econ_corr2$yearP[econ_corr2$yearP>2012 & econ_corr2$yearP<=2015]<-"2012-2015"
 
ggplot(data=econ_corr2, aes(x=unemployment_rate, y=X3month_AWEchange, colour=yearP))+
geom_point(size=3)+
theme_economist()+
scale_colour_economist()+
ylab("Average earnings % change")+
xlab("Unemployment rate %”)
{% endhighlight %}


[economist]: http://www.economist.com/news/britain/21688886-recent-strong-growth-britons-pay-packets-proves-short-lived-nice-while-it-lasted
[kieran]: http://kieranhealy.org/blog/archives/2016/01/16/two-y-axes/
[spurious]: http://www.tylervigen.com/spurious-correlations
[unemployment]: http://www.ons.gov.uk/ons/taxonomy/index.html?nscl=Unemployment#tab-data-tables
[average earnings]: http://www.ons.gov.uk/ons/taxonomy/index.html?nscl=Earnings#tab-data-tables

