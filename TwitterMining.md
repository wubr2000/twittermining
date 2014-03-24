Twitter Text Mining and Word Cloud Example for #MH370
========================================================

- First, access Twitter API. Run script and keep this open.

- If there is no error, `twitCred$handshake` will return a link. Copy and paste this link to the browser, then authorize the app.

- After clicking "Authorize app", **take note of the PIN and enter it directly inside the R Console**; finally, run the `registerTwitterOAuth(twitCred)` function and this should return "TRUE".


- Now make a word cloud for `#PrayforMH370` and `#MH370` within certain dates.


```r
library(twitteR)
```

```
## Loading required package: ROAuth
## Loading required package: RCurl
## Loading required package: bitops
## Loading required package: digest
## Loading required package: rjson
```

```r
library(tm)
library(wordcloud)
```

```
## Loading required package: Rcpp
## Loading required package: RColorBrewer
```

```r
library(RColorBrewer)

registerTwitterOAuth(twitCred)
```

```
## Error: object 'twitCred' not found
```

```r
mh370 <- searchTwitter("#MH370", since = "2014-03-08", until = "2014-03-24", 
    n = 1000)
```

```
## Error: OAuth authentication is required with Twitter's API v1.1
```

```r
mh370_text = sapply(mh370, function(x) x$getText())
```

```
## Error: object 'mh370' not found
```

```r
mh370_corpus = Corpus(VectorSource(mh370_text))
```

```
## Error: object 'mh370_text' not found
```

```r

tdm = TermDocumentMatrix(mh370_corpus, control = list(removePunctuation = TRUE, 
    stopwords = c("prayformh370", "prayformh", stopwords("english")), removeNumbers = TRUE, 
    tolower = TRUE))
```

```
## Error: object 'mh370_corpus' not found
```

```r

m = as.matrix(tdm)
```

```
## Error: object 'tdm' not found
```

```r
# get word counts in decreasing order
word_freqs = sort(rowSums(m), decreasing = TRUE)
```

```
## Error: object 'm' not found
```

```r
# create a data frame with words and their frequencies
dm = data.frame(word = names(word_freqs), freq = word_freqs)
```

```
## Error: object 'word_freqs' not found
```

```r

wordcloud(dm$word, dm$freq, random.order = FALSE, colors = brewer.pal(8, "Dark2"))
```

```
## Error: object 'dm' not found
```


- Words in high scale are likely to be Malay prepositions, or Malay `stopwords`.

- So look for a list of Malay stopwords online and store them:


```r
library(XML)

df1 <- readHTMLTable("http://blog.kerul.net/2014/01/list-of-malay-stop-words.html")
df1 <- df1[[1]]

malaystopwords <- as.character(unlist(df1))[-c(320, 321)]
```


- Supplying the previous code with the Malay stopwords:


```r
tdm = TermDocumentMatrix(mh370_corpus, control = list(removePunctuation = TRUE, 
    stopwords = c("prayformh370", "prayformh", malaystopwords, stopwords("english")), 
    removeNumbers = TRUE, tolower = TRUE))
```

```
## Error: object 'mh370_corpus' not found
```

```r

m = as.matrix(tdm)
```

```
## Error: object 'tdm' not found
```

```r
# get word counts in decreasing order
word_freqs = sort(rowSums(m), decreasing = TRUE)
```

```
## Error: object 'm' not found
```

```r
# create a data frame with words and their frequencies
dm = data.frame(word = names(word_freqs), freq = word_freqs)
```

```
## Error: object 'word_freqs' not found
```

```r

wordcloud(dm$word, dm$freq, random.order = FALSE, colors = brewer.pal(8, "Dark2"))
```

```
## Error: object 'dm' not found
```





