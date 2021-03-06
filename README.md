Twitter Text Mining
===================

- Using #MH370 as an example
- Cretaed a word cloud (over a specified period of time) based on hashtag #MH370


![Image](Wordcloud-MH370.png)


Twitter Text Mining and Word Cloud Example for #MH370
========================================================

- First, access Twitter API. Run script and keep this open.

- If there is no error, `twitCred$handshake` will return a link. Copy and paste this link to the browser, then authorize the app.

- After clicking "Authorize app", **take note of the PIN and enter it directly inside the R Console**; finally, run the `registerTwitterOAuth(twitCred)` function and this should return "TRUE".


- Now make a word cloud for `#PrayforMH370` and `#MH370` within certain dates.

```python
library(twitteR)
library(tm)
library(wordcloud)
library(RColorBrewer)

registerTwitterOAuth(twitCred)
mh370 <- searchTwitter("#MH370", since = "2014-03-08", until = "2014-03-24", n = 1000)
mh370_text = sapply(mh370, function(x) x$getText())
mh370_corpus = Corpus(VectorSource(mh370_text))
 
tdm = TermDocumentMatrix(
  mh370_corpus,
  control = list(
    removePunctuation = TRUE,
    stopwords = c("prayformh370", "prayformh", stopwords("english")),
    removeNumbers = TRUE, tolower = TRUE)
    )
 
m = as.matrix(tdm)
# get word counts in decreasing order
word_freqs = sort(rowSums(m), decreasing = TRUE) 
# create a data frame with words and their frequencies
dm = data.frame(word = names(word_freqs), freq = word_freqs)

wordcloud(dm$word, dm$freq, random.order = FALSE, colors = brewer.pal(8, "Dark2"))
```

- Words in high scale are likely to be Malay prepositions, or Malay `stopwords`.

- So look for a list of Malay stopwords online and store them:

```python
library(XML)
 
df1 <- readHTMLTable('http://blog.kerul.net/2014/01/list-of-malay-stop-words.html')
df1 <- df1[[1]]
 
malaystopwords <- as.character(unlist(df1))[-c(320, 321)]
```

- Supplying the previous code with the Malay stopwords:

```python
tdm = TermDocumentMatrix(
  mh370_corpus,
  control = list(
    removePunctuation = TRUE,
    stopwords = c("prayformh370", "prayformh", malaystopwords, stopwords("english")),
    removeNumbers = TRUE, tolower = TRUE)
    )

m = as.matrix(tdm)
# get word counts in decreasing order
word_freqs = sort(rowSums(m), decreasing = TRUE) 
# create a data frame with words and their frequencies
dm = data.frame(word = names(word_freqs), freq = word_freqs)
 
wordcloud(dm$word, dm$freq, random.order = FALSE, colors = brewer.pal(8, "Dark2"))
```
