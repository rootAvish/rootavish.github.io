---
title: "Metrics used in information retrieval"
date: 2024-01-07T10:01:38+05:30
draft: true
---

This is a short post that goes into a simple but important concept because the industry, especially
the NLP domain is full of IR usecases and seeing this metrics the first time throws one aback since
they're mostly of the form *prefix*-wellknown metric. Here I describe the task of information retrieval,
give a short summary of the pros and cons of different classical and neural IR approaches and then
directly jump into the different metrics and my intuition on when each should be considered.

This post is based on lectures notes from one of my classes at GT, so I would like to cite *Barlas Oguz*
as the inspiration for most of this knowledge and the framework.

## The task of information retrieval

Information retrieval is defined as the task of solving a user's *actual information need* given a user query
(their perceived information need). Think about this for a minute because this is a very important intuition
for understanding both why the metrics we will discuss are what they are, and has immense real life performance
implications for a system that is deployed to production as in those cases even when you correctly respond to queries
the user's information need is not satisfied.

Take the case of a system where a knowledge base contains FAQs around how to perform a certain task and when the user
asks a question, you respond with the context of the FAQ in response to the user's perceived information need. However,
it is usually a problem with such systems that the actual information need is one of the users wanting to know information
that is specific to their particular case, the answer to which lives in some backend system somewhere and the FAQs
answer at 10k feet and present a procedural response.

In the above example, the actual information need is unsatisfiable, however unless the user is explicit with it there's no
way for the query system to know that's the case without using some sort of query enhancement.


## Boolean Relevance

The idea of boolean relevance is that quite simply, a document is relevant when it satisfies the user's *actual* information need
and irrelevant when it does not satisfy the same. The advantage of vieweing things in black and white like this has the advantage
that it is simple enough to explain up the chain, but at the same time hides nuance around ranking, i.e. is there a document
that carries higher relevance to the user's query than the one that we returned.

While the metric is black and white, annotating for it is not since that will be done by a human labeler looking at the query
(perceived information need) and it is non-trivial to arrive at the actual information need for the same. It is safe to assume
that was the actual information need fully spelled out, the IR system itself will then be the bottleneck.

## Metrics

First let's categorise and layout the different metrics that can be used to evaluation an information retrieval system.

### Fundamental

* Precision@k: What fraction of top k documents are relevant.
* Recall@k: Number of documents relevant to the user present in the top k.


## Glossary

Here I include some glossary that you will come across in all IR literature, and is therefore relevant to the study of IR metrics
as well.

- *Term Document Incidence Matrix*: This is basically the Matrix that you'd get as the output of CountVectorizer with `binary` set
to True. It can be used directly to perform Boolean retrieval. It is highly sparse.

- *Inverted Index*: It is the bread and butter of all IR. The idea of "inversion" is simply that instead of having an index into the
table via document ids and returning all terms corresponding to that document, you index into a structure with words and fetch
all documents that contain that word. Simple retrieval would then be an intersection of all sets retrieved from the terms of a query.
    * It can also be used to lookup the term frequency which it stores, but that's an implementation detail since scipy nnz on a sparse
matrix caches this for all its rows.
    * Each of its rows is called a "posting list".
    * It can also at times be extended to include positional information for phrase matching.

- *Ranked Retrieval*: Fundamentally, retrieval is mostly not black and white because even if you include all relevant document in a set of search results, it is vital to get the ordering of those results correct. Additionally, in boolean relevance there is no "shades of grey" or in other words "somewhat relevant".

- *Collection frequency* - Number of times a word appears in a corpus.

- *Document frequency* - Number of documents that contain a word.
