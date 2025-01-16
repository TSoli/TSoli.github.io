---
title: "Bag-of-words: A Simple Statistical Representation of Natural Language"
date: 2024-11-05
categories: [AI, Natural Language Processing]
tags:
  [
    study,
    natural language processing,
    machine learning,
    artificial intelligence,
    bag-of-words,
    statistics,
    statistical learning,
  ]
math: true
toc: true
---

<!-- prettier-ignore -->
* TOC
{:toc}

## Documents Are Just the Sum of Their Words

Well at least that's kind of the assumption for bag-of-words models. I'll talk about some potential
limitations later but first let me explain how this method works.

The aim here is to create a representation for documents in computers that can be efficiently used
for tasks such as comparison or matching with a query. _Document_ is used loosely here to describe
some discrete unit of natural language text for the purposes of whatever task it is used for. For
example, if we wanted to find relevant passages in a textbook given a query, we might make each
paragraph a _document_. Of course, if I search with a question like _What is Newton's Second Law of
Motion?_ then we need some way of representing the query and the paragraphs in the textbook such
that an algorithm could find and sort the most relevant paragraphs.

## Bag-of-words

### Term-Document Matrix

The idea for bag-of-words models is to disregard the order of words and represent document based
only on the frequencies of the terms in them. Hence we can think of it like throwing all of the
words into a bag and mixing them up - a _bag-of_words_. For example, we could create a term-document
matrix for some of the phrases in Dr Seuss's _Green Eggs and Ham_.

> P1: _WOULD YOU LIKE THEM HERE OR THERE?_\
> P2: _WOULD YOU LIKE THEM IN A HOUSE?_\
> P3: _I DO NOT LIKE THEM IN A HOUSE._\
> P4: _I DO NOT LIKE THEM HERE OR THERE._\
> P5: _I DO NOT LIKE GREEN EGGS AND HAM._

Different models may make different decisions about how the words are split (known as tokenisation)
but for this example I will just ignore punctuation and split word-by-word.

<div align="center" markdown="1">

|       | P1  | P2  | P3  | P4  | P5  |
| ----- | --- | --- | --- | --- | --- |
| WOULD | 1   | 1   | 0   | 0   | 0   |
| YOU   | 1   | 1   | 0   | 0   | 0   |
| LIKE  | 1   | 1   | 1   | 1   | 1   |
| THEM  | 1   | 1   | 1   | 1   | 0   |
| HERE  | 1   | 0   | 0   | 1   | 0   |
| OR    | 1   | 0   | 0   | 1   | 0   |
| THERE | 1   | 0   | 0   | 1   | 0   |
| IN    | 0   | 1   | 1   | 0   | 0   |
| A     | 0   | 1   | 1   | 0   | 0   |
| HOUSE | 0   | 1   | 1   | 0   | 0   |
| I     | 0   | 0   | 1   | 1   | 1   |
| DO    | 0   | 0   | 1   | 1   | 1   |
| NOT   | 0   | 0   | 1   | 1   | 1   |
| GREEN | 0   | 0   | 0   | 0   | 1   |
| EGGS  | 0   | 0   | 0   | 0   | 1   |
| AND   | 0   | 0   | 0   | 0   | 1   |
| HAM   | 0   | 0   | 0   | 0   | 1   |

</div>

Along the columns we can see the different documents and along the rows the different words or
terms. Note that in this case, each document has only a maximum frequency of one and therefore the
numbers are only 1 or 0 whereas normally they could be much larger. These relationships can help us
compare words or documents. For example, if we compare rows, the assumption is that similar words
will have similar frequencies in each document. Alternatively, we could compare columns where we
would assume similar documents contain similar frequencies of words. In general, this term-document
matrix has $|V|$ rows and $|D|$ columns, where $|V|$ is the number of words in the vocabulary and
$|D|$ is the number of documents.

### Term-Term Matrix

Alternatively, we could create a term-term matrix where both the rows and columns are the words in
the vocabulary. Now each cell would represent the number of times the words co-occurred within some
context window. For this example, I will use each line as the context window. The resulting matrix
would be $|V| \times |V|$ which even for this case starts to become large. Therefore, I will just
show a subset of the matrix below.

<div align="center" markdown="1">

|     | DO  | YOU | LIKE |
| --- | --- | --- | ---- |
| I   | 3   | 0   | 3    |
| YOU | 0   | 2   | 2    |

</div>

The term-term matrix allows us to compare words with the assumption that similar words will be used
in similar contexts and so they would share similar rows/columns. The example above is mostly an
example of how to construct the matrix from some data but due to the small amount of data it is
harder to show this relationship. As a better example, I will use the table below taken from _Speech
and Language Processing_ by Jurafsky and Martin. The table presents a term-term matrix for some
words in the Wikipedia corpus.

![A term-term matrix showing co-occurrence of some words in the Wikipedia corpus](/images/term-term_matrix_jurafsky_martin.png)_A
term-term matrix showing co-occurrence of some words in the Wikipedia corpus_

Here we get some idea that the words _digital_ and _information_ are similar since they have high
co-occurrences with _computer_ and _data_ and low co-occurrence with _pie_ and _sugar_. We can also
deduce that cherry and strawberry are semantically similar due to high co-occurrence with _sugar_
and _pie_ but low co-occurrence with _computer_ and _data_. Furthermore, we can conclude that
_digital_ and _information_ are quite semantically dissimilar to _cherry_ and _strawberry_ since
they do not share large co-occurrences for any of the words explored.

## Similarity Metrics

### Words and Documents as Vectors

So now that we can see how these bag-of-words approaches, how can we determine which words or
documents are similar? Well, as I have alluded to, we can compare rows or columns that represents
words or documents. More formally, we can treat these rows or columns as high dimensional vectors
where each frequency is a component. The vocabularies used usually have thousands of words and
therefore, there are many words which may have extremely low or 0 co-occurrences in the dataset. As
a result, these representations are often referred to as sparse vector representations.

### Dot Product

Now that we have a representation for words and documents as vectors, a natural choice would be to
use vector similarity metrics to compare document vectors or words to other word vectors. The first
that might spring to mind then is the dot product which is defined.

$$
\mathbf{a} \cdot \mathbf{b} = \lVert \mathbf{a} \rVert \lVert \mathbf{b} \rVert \cos\theta = \sum^i a_i b_i
$$

where $\theta$ is the angle between the two vectors and $a_i,\,b_i$ are the components of each of
the vectors. The consequence of the second representation of the dot product is that we multiply
frequencies related to the same words together. Therefore, documents that contain high frequencies
of the same words or words that co-occur with the same words will have overlap that increases the
resulting dot product.

### Cosine Similarity

An undesirable side-effect of directly using the dot product is that longer documents or words that
occur more frequently will naturally have higher frequencies of words (or co-occurrences) and
therefore have higher dot products on average. Of course, the length of the document or frequency of
the word should not have any bearing on semantic similarity. Therefore, to correct for this, we
could instead look at the relative angles of the vectors. The angles are determined by the relative
magnitudes of each of the components (or frequencies in this case) and therefore, documents with
similar relative frequencies of words will have similar directions. Intuitively, this makes sense
since we would expect documents with similar meanings to have similar relative frequencies of words.
Similar directions will result in angles closer to 0 and so if we take the cosine of this angle
instead, then the result is conveniently between -1 (for vectors in the exact opposite direction)
and 1 (for vectors in the exact same direction). Furthermore, the components of these semantic
vectors are all non-negative frequencies which restrict the range of angles to between $-\pi/2$ and
$\pi/2$. The resulting cosine of these angles is then between 0 (for completely orthogonal) and 1
(exactly the same direction). The cosine of the angle between the two vectors can be conveniently
calculated by rearranging the dot product equation.

$$
\cos\theta = \frac{\mathbf{a} \cdot \mathbf{b}}{\lVert \mathbf{a} \rVert \lVert \mathbf{b} \rVert}
$$

### TF-IDF

Common words like _the_, _a_ or _it_ occur in high frequencies in many documents and co-occur
frequently with most words. This is a problem because these ubiquitous words artificially increase
the similarity of all words if using the dot product or cosine similarity. tf-idf attempts to
correct for this effect.

The metric is often used for documents and consists of two parts, the term frequency (tf) and the
inverse document frequency (idf). The term frequency aims to increase the score for documents with
high frequencies of the target word while the inverse document frequency aims to decrease the score
for words that appear in lots of documents and are therefore not very discriminative. These two
components are then usually multiplied together to produce the tf-idf score.

It is important to note that there are many variants of tf-idf. One formulation is to take the log
of the raw frequencies so that the tf term is,

$$
\text{tf}(t, d) =
\begin{cases}
  1 + \log f_{t,d}, & \text{if}\ f_{t,d} > 0 \\
  0, & \text{otherwise}
\end{cases}
$$

where $f_{t,d}$ is the frequency of the term $t$ in the document $d$. The justification for taking
the $\log$ is that the frequency of the word does not linearly correlate with its importance to the
meaning. Instead, the presence should indicate some amount of importance and further occurrences
should have a diminishing impact as is the case with the $\log$ function.

Similarly, the idf can be defined,

$$
\text{idf}(t, D) = \log \frac{N}{n_t}
$$

where $N$ is the total number of documents and $n_t$ is the number of documents that contain the
term $t$. The overall metric can then be defined,

$$
\text{tf-idf} = \text{tf} \times \text{idf}
$$

One slight limitation of this variant is that it does not correct for the overall length of
documents. Due to the logarithm this effect is not massive however alternatives exists that define
tf by dividing the raw frequency of the term by the length of the document or the frequency of the
highest frequency word in the document.

## Application: Information Retrieval

A common application of these algorithms is for information retrieval with natural language text.
For example, when you perform a search on the Internet using a search engine, these algorithms could
be used to find and rank documents with high similarity scores to the query. It is important to note
that these similarity metrics are only a heuristic for relevance and it's completely possible that
relevant documents for a query has words that do not predominantly feature in it.

Furthermore, there are of course other popular similarity metrics such as
[Okapi BM25](https://en.wikipedia.org/wiki/Okapi_BM25) which are designed specifically for
information retrieval and have been used by popular Internet search engines with modifications.
Despite this, it is no longer the state-of-the-art and neural techniques have become more popular.
This will be a topic for a future post.

## Have We Truly Encoded Meaning?

I am by no means an expert in this field and the following is just my opinion based on my limited
knowledge. With that said, despite the effectiveness of these bag-of-words approaches, I think it is
hard to argue that the vectors used to represent documents and words truly encode meaning. They seem
like useful statistical representations however, the true meaning of words and documents obviously
should depend on their order and context. Of course, practically this isn't an issue if it can be
used to reliably find documents that meet an information need and these methods have been fairly
effective at doing this. This distinction probably only matters in more advanced natural language
tasks and explains why the current generative models use more complex techniques to generate
convincing text that _do_ take into account the order of text.

## More Information

If you want more information about these techniques, see _Speech and Language Processing_ by
Jurafsky and Martin. Additionally, you can have a look at my [other
post]({% post_url 2024-11-04-word2vec %}) which discusses how neural networks were able to provide
more convincing semantic encodings for words with Word2vec. edit
