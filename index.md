# Wait a second... How do we detect arab characters?

Indeed, an important stepping stone for the analysis is to have a consensus on what do we mean by "Arab" character or location. For this purpose, we hand-crafted a list of arabic names, using tools such as wiki. Since the dataset provides a list of characters and a plot, two methods were used for detection: either directly compare the names in the list with the provided characters in the dataset, or use a Named Entity Recognition (NER) framework (such as spaCy model) to extract a list of names from the plots, and then compare that list with our reference list for arabic names.

To verify the quality of this list, we prompted the chatGPT large language model to identify movies with arabic characters by hand feeding it a randomly selected subset of movie 

*explain use of chatGPT word list, filtering + muslim VS arabic* --> + matteo's branch 
