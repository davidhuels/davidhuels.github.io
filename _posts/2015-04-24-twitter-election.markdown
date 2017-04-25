---
layout: post
title:  Twitter analysis for the General Election
date:   2015-04-24
categories: jekyll update
---
The general election in the UK is approaching. We all have seen in the last US elections how Obama used social media (i.e. facebook and twitter) for his campaign.
To my surprise in the UK any political advertisment in TV or radio is banned (in contrast to other European countries). So it is not surprising that all the political parties make heavy use of twitter to get their message across.
I thought this would be an ideal opportunity to try some R code to analyse what they are saying. Once I’ve done that I thought it would be fun to see if we could even cluster the main candidates together depending what they are talking about.


First let’s have a look at their twitter messages. For that I I analalysed the words they used and how often. According to the frequency of the word, you can draw a wordcloud. The height of the word is determined by it’s frequency. For Ed Miliband, David Cameron and Nick Clegg I was able to analyse more than 200-600 of their latest tweets. For the other party candidates (Nigel Farage, Natalie Bennett, Jen Wood and Nicola Sturgeon) I only got about 30-50 tweets (not sure if they just tweet less?), therefore I didn’t include the wordcloud from these candidates (will update as soon as I find a way to get more tweets from them).

Ed Miliband
![miliband](/assets/twitter_election/mili2.png){:class="img-responsive"}

David Cameron
![cameron](/assets/twitter_election/cameron2.png){:class="img-responsive"}

Nick Clegg
![clegg](/assets/twitter_election/clegg2.png){:class="img-responsive"}

The fun part is to see if the computer can cluster certain candidates together depending what they are talking about. For that we use the Biologist’s favourite: the heatmap.
![heatmap](/assets/twitter_election/twitter_heatmap3.png){:class="img-responsive"}

I created a list of words consisting of the top10 words from each candidate. Each row represents a word, the colour of the cell is the relative frequency from that candidate (blue low – red high). With hierarchical clustering we can now group the candidates together by the colour of the cells, so how similar are they in terms of frequency of a topic. The dendrogram at the top shows the tree like relationship. We can see it divides the candidates into two major subgroups: Ed Miliband and David Cameron on one side, the other candidates on the other side. So we can immediately see that Ed Miliband and David Cameron are discussing the same topics. Interestingly, Nick Clegg didn’t make it in that group. He is together with the candidates from the smaller parties but splits early enough. For the other candidates we have to be a little bit careful, since here I only analysed a few tweets. They are in the other branch, next to Nick Clegg and are quite similar. But they are quite similar in terms that they are talking about their specific topics, which are mainly unique important to them. So, for example no one else is talking about UKIP, but Nigel Farage. Surprisingly “immigration” was not in his top10 words.
I think this might even change over the next couple of days, so it will be interesting to run the same code at a later stage.
Overall it is interesting to see, that we can distinguish the candidates by their tweets and it is clear from that who is in the race to become prime minister and who is not.

{% highlight R %}
library(twitteR)
library(RCurl)
library(tm)
library(wordcloud)
library(gplots)
library(RColorBrewer)
 
#identification for the twitter API, you need to make your account
setup_twitter_oauth("xxxxxx", "xxxxxx",
                    access_token="xxxxxxxxxxx",
                    access_secret="xxxxxxxxxx")
 
 
twitter_wordfreq<-function(person){
  #get twitter data from timeline of person
  timeline<-userTimeline(person, n=1000)
  #print total tweets retrieved
  print(paste("Tweets:", length(timeline), sep=""))
  #convert to dataframe
  df <- do.call("rbind", lapply(timeline, as.data.frame))
  myCorpus <- Corpus(VectorSource(df$text))
   
  myCorpus <- tm_map(myCorpus, content_transformer(tolower))
  # remove punctuation
  myCorpus <- tm_map(myCorpus, removePunctuation)
  # remove numbers
  myCorpus <- tm_map(myCorpus, removeNumbers)
  # remove stopwords
  # including ampersand(amp) = &
  myStopwords <- c(stopwords('english'), 'available', 'via', 'amp')
  #remove URLs
  removeURL<-function (x) gsub("http[[:alnum:]]*", "", x)
   
  myCorpus<-tm_map(myCorpus, content_transformer(removeURL))
  myCorpus <- tm_map(myCorpus, removeWords, myStopwords)
  #building a document term matrix
  myTdm <- TermDocumentMatrix(myCorpus, control = list(minWordLength = 1))
   
  #prepare word frequency data frame
  m <- as.matrix(myTdm)
  # calculate the frequency of words
  v <- sort(rowSums(m), decreasing=TRUE)
  d <- data.frame(word=names(v), freq=v, row.names=NULL)
  colnames(d)<-c("word", person)
  return(d)
}
 
#get tweets from each politician
mili<-twitter_wordfreq("ed_miliband")
cameron<-twitter_wordfreq("david_cameron")
clegg<-twitter_wordfreq("nick_clegg")
bennett<-twitter_wordfreq("natalieben")
nigel<-twitter_wordfreq("nigel_farage")
sturgeon<-twitter_wordfreq("nicolasturgeon")
wood<-twitter_wordfreq("leannewood")
 
#to make a wordcloud
wordcloud(mili$word, mili$ed_miliband, random.order=FALSE, colors=brewer.pal(8, "Dark2")) 
 
#make a list of top10 words for each politician...not the most efficient way
x1<-as.character(mili$word[1:10])
x2<-as.character(clegg$word[1:10])
x3<-as.character(cameron$word[1:10])
x4<-as.character(bennett$word[1:10])
x5<-as.character(nigel$word[1:10])
x6<-as.character(sturgeon$word[1:10])
x7<-as.character(wood$word[1:10])
urlist<-data.frame(c(x1, x2,x3, x4, x5, x6, x7))
colnames(urlist)<-"word"
#remove duplicates in urlist
urlist<-unique(urlist)
 
#merge word freq of each politician with urlist
merge1<-merge(x=urlist, y=mili, by="word", all.x=TRUE)
merge12<-merge(x = merge1, y = cameron, by = "word", all.x = TRUE)
merge123<-merge(x = merge12, y = clegg, by = "word", all.x = TRUE)
merge1234<-merge(x = merge123, y = bennett, by = "word", all.x = TRUE)
merge12345<-merge(x = merge1234, y = nigel, by = "word", all.x = TRUE)
merge123456<-merge(x = merge12345, y = sturgeon, by = "word", all.x = TRUE)
merge_compl<-merge(x= merge123456, y = wood, by="word", all.x=TRUE)
 
#substitute NAs for 0 and save
merge_compl[is.na(merge_compl)] <- 0
write.csv(merge_compl, file="election_twitter3.csv")
 
 
# transform column 2-5 into a matrix
twitter_matrix <- data.matrix(merge_compl[,2:8]) 
# assign labels in column 1 to "rownames"
rownames(twitter_matrix) <- merge_compl[,1]               
 
head(twitter_matrix)
#png("twitter_heatmap.png", width = 5*300, height = 5*300, res = 300, pointsize = 8) 
heatmap.2(twitter_matrix, trace="none", col=bluered(75),
          cexCol=1, margins=c(4,8), density.info="none",
          keysize=1, key.xlab="rel. word frequency",
          srtCol=30, dendrogram="column", scale="column")
#dev.off()
{% endhighlight %}

