---
layout: page
title: Representation of Arabs in cinema before and after 2001
cover-img: "images/dictator_ada.jpeg"
subtitle: Unraveling the Reel Impact of Real-World Events on Arab Representation in Movies
---

In the captivating realm of cinema, the silver screen not only reflects our collective imagination but also mirrors the complex tapestry of societal changes and historical events. One such moment that reverberated globally was the tragic day of September 11, 2001. Beyond its immediate and profound impact on geopolitics, security measures, and the psyche of nations, the events of 9/11 had a far-reaching influence on the cultural landscape, including the portrayal of Arab identities in the movies.

Our data analysis journey delves into the captivating intersection of reality and reel, seeking to understand how one of the most significant events of the 21st century shaped the representation of Arabs in cinema. The lens through which movies depict Arab characters, cultures, and narratives has often been subject to scrutiny, with questions raised about the perpetuation of stereotypes and the impact of real-world events on cinematic storytelling.

Through our analysis of the CMU movie dataset, spanning pre and post-9/11 eras, we hope to unearth patterns, trends, and anomalies that illuminate the multifaceted relationship between historical events and cinematic narratives. As we unravel the *reel impact of real-world events* (*tu-dums*), our journey promises to uncover not only the challenges but also the opportunities for more authentic storytelling in the world of cinema. So, grab your popcorn, and.... *Lights... Camera... Action!*

# *"Nice to meet you, I'm Data"*: getting familiar with the dataset

![my image](images/dictator_ada.jpeg)

recycle P2, facts and graphs about initial pre-processings

# Wait a second... How do we detect arab characters?

Indeed, an important stepping stone for the analysis is to have a consensus on what do we mean by "Arab" character or location. 

*explain use of chatGPT word list, filtering + muslim VS arabic* --> + matteo's branch 


# Wait another second... What year range to use?
The selected years for the study need to have a consistent rapresentation of arabs before and after 2001 and exclude other non-negligible real-world events. We thus limit the analysis to movies from 1972 to 2012. We know that there are still historical events that could have changed the arab rapresentation, as the Gulf war in 1990. Still, we think this is a good compromise between the number of film analysed and the other _noisy_ previuns historical events. Speking about events after 2001, we can say that lot of them were triggered by the 9/11 attack, the so called "War on Terror" announced by President Bush, that led to Afghanistan and Iraq wars.

# [...] the crime and war genre

A first question we can ask is how did the participation of Arab characters in the crime and war (c&w) genre change after 2001, if it changed? Is this effect more evident for the western countries? This analysis can be made without the use of a movie's plot so it is a great place to start at. Here, we detect arab characters in our dataset directly from the character metadata.  Let's see if some trends can be spotted immediately by plotting simple statistics at first, regarding all character parteciparion and arab character partecipation in w&c movies!

![yearly characters for w&c](images/yearly_ch_w&c.png)


< Percentage of characters (overall and arab) in Crime and War movies, per year > 

- Considering all the characters (blue bars), We can see that there is no clear trend. After 2003 the value seems eventually to decrease, that means a decresing partecipation for characters in w&c movies.

- Arabs (red bars) show a lot of fluctuations during the years, evidently during the last century. This is due to the small partecipation in W&C (less than 5 per year before 2003), generating less reliable percentages. The trend seems to have a local increase between 2002 and 2006. After 2004 arab characters seems to play a higher fraction of these genre films than the overall characters in terms of mean values. Still the uncertainty given by bootstrapping shows that this can be stated only for years 2006, 2011 and 2012, where the confidence intervals do not intersect.

In parallel, we filter for films pubblished in western countries, to understand if there is a more evident effect:

![yearly characters for w&c, western countries](images/yearly_ch_w&c_we.png)
The western country time evolution is really similar respect to the all countries case. Accounting for all characters, we notice again a stagnant trend with a decrease after 2003. Arabs show a more evident lack of data before 2000 with large confident interval. Still, we can see the local increase in the interval 2002-2006.

To be more precise on this analysis we look at the feature before and after 2001. This grouping is done to avoid the problem of sparsity of data for arabs, specially doring the last century movies, that leads to high uncertainty.
We want also to check that a possible increase of arab roles, a subset of the roles, is not due to the increase of the all character partecipation. 


![Coefficients before balancing](images/Coefficients_before_bal.png)
The linear regression gives significant results in all the 4 cases.
For both all countries and the Western subset we see a negative coefficient looking at the charachìters in general (yellow dots). It means a decrease of partecipation in w&c movies comparing the ones pubblished before and after 9/11. On the contrary, Arabs show a positive correlation (green dots), but we cannot say that the one for wester countries is higher since the error bars intersect.  

The positive correlation for the arabs could be also caused by confounders.  To investigate the causal link, we use an observational study scheme, with the treatment as the pubblication after 2001 and the outcome as the fraction of arab characters in w&c movies. This is done for both the whole world and the western contries, generating two studies.
Also, to say that the trend for arabs is not due to a general increase of the all characters trend, we should set another observational study and compare the output coefficients. This is out of the scope of our analysis, that uses the results showed before as a first prove on this.

### Observational study on w&c movies

Are we ready for some causal diagrams and confounding factors? As mentioned, here we go with 2 sweet observational studies! Whereas for the first we use all the country of pubbliaction, for the `second` we filter for the western countries. 
We have the following treatment (**T**) and outcome (**O**)
- **T** Arab character participates in a film released after 2001 (`published in a western country`)
- **O** Movie belongs to a w&c genre


First of all, we need to discuss the possible confounders (**C**) of the causal analysis. We treat the following features:
- **C1** Geographical zone of the movie pubblication
    - effect on the treatment: cinema industy can be more or less active in a country depending on cultural and economic effect that may change during the years. Es: boom of the indian industry after 2000
    - effect on the outcome: different countries have different cultures, so different incidence on a certain genre. es: a c&w film with arab caracters could be pubblished more probably in the US
- **C2** Sex of the actor: 
    - effect on the treatment: arab females could be obstacled to be actress for religious rules that were stricter in the past, so there could be less female arab characters in the past 
    - effect on the outcome: male characters could be used more frequently in w&c films
- **C3** Age of the actor at release 
    - effect on the treatment: during the film history it is possible that the presence of young actors was obtacled by certain legal/cultural rules
    - effect on the outcome: young characters and therefore actors could have a role more frequently in w&c films

The informations on sex and age are simply obtainable from the characters metadata. For the region we used the information on of Movie countries from the Movie metadata grouped the coutries following the [United Nations](https://ourworldindata.org/world-region-map-definitions). For the second observational study **C3** is not a possible confounder since all the western countries, where we hypotyse that the cultural influence is similar, are included in the region "Europe and North America".

![United nations regions](images/world-regions-sdg-united-nations.png)


Two skemes sums up the hypotised causal links stated before:
![causal diagrams](images/Causal_diagram.png)
< present OLS results >  + western filter

< present observationnal study results > + western filter

To conclude this analysis, we can say that there is a significant increase in participation of arab characters in w&c movies, [intensified when accounting only for western countries NOT SIGNIFICANT CF BAR PLOT], which intuitively, makes sense. But this result does not consider the possible positive / negative connotations in the movie. In the following parts, we delve in this more nuanced analysis.



# Sentiment analysis
Let's now dive into the core of our study: sentiment analysis. Sentiment analysis is the process of analyzing digital text to determine if the emotional tone of the message is positive, negative, or neutral. Our objective here is to understand if there is a significant change of sentiment towards arabic characters when depicted in US and Europe movies (i.e. "Western countries"). 

As we want to be sure that the sentiment analysis we perform is correctly related to the arabic characters, we perform this analysis on a range of `n` words before and after the target name (local contexts). 
As an example, let's consider the movie "House party 2" (1991). Here's an extract of the movie summary, highlighting the detected arabic character.

_"Kid and Play get into a fight and **Bilal** then convinces Kid to ask Sydney for money. Kid tries to approache Sydney but Sydney assumes that he wants to break up with her."_

For example, if `n = 5`, the following chunk of plot summary will be considered as the context around the arabic name _Bilal_.

_"Play get into a fight and **Bilal** then convinces Kid to ask Sydney"_

To determine the optimal context window size for sentiment analysis, we conducted a manual exploration of hyperparameters (hyperparameter tuning). This involved iteratively trying different sizes and examining example outputs to assess the impact on sentiment analysis results. By visually inspecting the sentiment context around Arabic names in sample plots, the most effective context window size was identified as `n = 9`. 

All of this said, let's have a look at the evolution of sentiment throughout the years.

To perform sentiment analysis and get sentiment polarity scores, VADER model is used. This model evaluates both polarity (positive/negative) and intensity (strength) of emotion of each word in a text. Then, it adds them up to obtain the sentiment score of the entire body. The model distinguishes 5 sentiment polarities:
- positive
- negative
- neutral
- compound

The compound score is the sum of positive, negative & neutral scores which is then normalized between -1 (most extreme negative) and +1 (most extreme positive).

![my image](images/mean_sentiment_year_pos_neg.png)

![my image](images/mean_sentiment_year_comp.png)

The evolution year-by-year does not show us with a clear change of trend before and after 2001. The confidence intervals are almost always overlapped and not much can be stated even for the difference between positive and negative sentiment.

Let's now cumulate the sentiment scores of the range of years $[1972,2001]$ and $(2001,2012]$.  We can perform a t-test between the two distributions, respectively for compound, positive, and negative sentiment, obtaining the following results.

- The p-value for the compound sentiment scores is: 0.045
- The p-value for the positive sentiment scores is: 0.031
- The p-value for the negative sentiment scores is: 0.56

These results mean that there is a significant difference between the compound and positive sentiment distribution with a significance level of $\alpha = 0.05$. We can thus look into the distributions.

<div style="text-align:center">
  <img src="images/distributions_pos_neg.png" alt="my image" >
</div>

<div style="text-align: center;">
  <img src="images/distributions_comp.png" alt="my image" style="width: 50%;">
</div>


As it is visible from these plots, positive sentiment scores after the year 2001 are consistently below the correspondat before 2001. On the other hand, negative sentiment scores after 2001 are consistently higher than before 2001 (even though this difference cannot be considered significant from the t-test result). The compound plot summarizes these results, as it is clear how a negative compound score (negative sentiment) is dominated by "after 2001" and a positive compound score (positive sentiment) is dominated by "before 2001".

Let's now visualize the most common words for the local contexts around arabic characters, via a wordcloud representation.

<div style="text-align:center">
  <img src="images/wordcloud.png" alt="my image" >
</div>


It is interesting to notice how some semantically negative words increase theire frequency after 2001 (kill, bomb). However, it is difficult to actually appreaciate a big difference between the two situations with such a representation. 
