--- 
title: "Sentimientos en informes de estabilidad financiera"
author: "[Luis Ortiz-Cevallos](https://ortiz-cevallos.github.io/MYSELF/)"
date: "2022-07-07"
---

# INTRODUCCIÓN

El análisis de sentimiento te ayuda extraer de un texto el estado de animo del editore de ese texto.

Por ejemplo, a partir de un texto que contenga la conversación entre dos personas podemos usar la función (`qdap's polarity()`) la cual califica el texto en dos escalas: **positivas y negativas**. 

Noten que para aplicar esa función los input son:
+   simples carácteres o dataframe
+   agrupamiento de palabras

En tanto el output es un objeto de la clase polaridad 



## El text mining con bolsas de palabras

**Corpus** es un conjunto de texto a partir del cual se pueden aplicar varios procesos.

La función (`VectorSource()`) pasa un vector de carácteres a una fuente de texto. A continuación un ejemplo:

La función (`VCorpus()`) pasa una fuente de texto a un Corpus.

Luego es importante limpiar tu Corpus. con funciones como: (`removePunctuation() y stripWhitespace()`) del paquete (`tm`) y la función 
(`replace_abbreviation()`) del paquete (`qdap`)


```r
library(qdap)
library(tidyverse)
library(pdftools)
library(stringr)
library(tm)
#pdf.text <- pdf_text("muestra.pdf") 
#write.table(pdf.text, "muestra.txt", sep=";")
pdf.text2 <- pdf_text("muestra.pdf") %>% str_split("\n") 
pdf.text2<-unlist(pdf.text2)
write.table(pdf.text2, "muestra.txt", sep=";")
tm_define<-read.csv("muestra.txt")
tm_define<-tm_define[1,1]
tm_define
```

```
## [1] "1;Text mining is the process of distilling actionable insights from text."
```



```r
tm_vector <- VectorSource(tm_define)
tm_corpus <- VCorpus(tm_vector)
content(tm_corpus[[1]])
```

```
## [1] "1;Text mining is the process of distilling actionable insights from text."
```


```r
clean_corpus <- function(corpus){
  corpus <- tm_map(corpus, content_transformer(replace_abbreviation))
  corpus <- tm_map(corpus, removePunctuation)
  corpus <- tm_map(corpus, removeNumbers)
  corpus <- tm_map(corpus, removeWords, c(stopwords("en"), "coffee"))
  corpus <- tm_map(corpus, content_transformer(tolower))
  corpus <- tm_map(corpus, stripWhitespace)
  return(corpus)
}
tm_clean <- clean_corpus(tm_corpus)
content(tm_clean[[1]])
```

```
## [1] "text mining process distilling actionable insights text"
```
### An unnumbered section {-}

