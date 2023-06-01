# ATLANTIS Dataset
This repository contains the ATLANTIS (coAsTaL mAritime NavigaTion InstructionS) dataset files.

This work is co-financed by the Shom and the IGN and is being carried out at the LASTIG, a research unit at Université Gustave Eiffel.

Under no circumstances should any part of this work be used for real-life navigation.

## Overview
ATLANTIS is a French-language dataset for nested spatial entity and binary spatial relation extraction from text. The dataset is composed of extracts from 15 different volumes of the _Instructions nautiques_. The _Instructions nautiques_ are a series of books produced and published by the French Naval Hydrographic and Oceanographic Service, the [Shom](https://www.shom.fr/). Each volume contains essential information for navigating safely in the coastal waters of a specific geographic area, including instructions for entering ports and descriptions of the coastal maritime environment. We extracted the text from the PDF documents using [pdfminer.six](https://github.com/pdfminer/pdfminer.six). The volumes, which are written in French, cover coastal areas in Africa, Europe, North and South America, as well as in the Indian and Pacific Oceans.

## Annotation Scheme
We annotated our dataset by hand using the [brat rapid annotation tool](http://brat.nlplab.org), which allows creating nested labelled annotations and creating directed labelled links between them. The source text was split on whitespace by brat, giving a dataset of 101400 tokens.

### Nested Spatial Entities
We use three labels to annotate unnamed and named nested spatial entities:
- **geographic feature** refers to common nouns that represent types of spatial entities
- **name** refers to pure proper nouns
- **geographic name** refers to the full name associated with a geographic feature

A token cannot be annotated only with the **name** label. A token cannot be annotated with both the **geographic feature** label and the **name** label. Any token can be annotated zero or more times with the **geographic name**.

### Binary Spatial Relations
We use 19 labels to annotate binary spatial relations:
- **is off the coast of** refers to an entity that is off the coast of another (it is always referred to using the same words in French in our dataset: ``au large de'')
- **is marked by** refers to an entity that is marked by another (usually a sea mark such as a buoy or a beacon)
- **is an element of** refers to an entity that is situated on or in another such that a bird's eye view shows the spatial footprint of one as being within or partly within the other
- **is [direction] of** refers to an entity that is in any one of 16 cardinal directions with respect to another: the 16 relations include four that use the cardinal directions (N, E, S, W), four that use the intercardinal directions (NE, SE, SW, NW) and eight that use the secondary intercardinal directions (NNE, ENE, ESE, SSE, SSW, WSW, NWN, NNW)

All relation annotations must link two entity annotations with the exception of **name** entity labels, which cannot be part of relation annotations. All relation annotations must have a direction.

## Repository Content
The [atlantis_dataset_brat](atlantis_dataset_brat) directory contains the .txt source files and the corresponding .ann annotation files for the entire dataset.

The [atlantis_dataset.jsonl](atlantis_dataset.jsonl) file contains the entire annotated dataset in a JSONL format. The specific JSONL format is that of the [SCIERC dataset](http://nlp.cs.washington.edu/sciIE/). We used [this script](https://github.com/dwadden/dygiepp/blob/master/scripts/new-dataset/brat_to_input.py), modified to use the [fr_core_news_sm](https://spacy.io/models/fr#fr_core_news_sm) spaCy model for tokenisation, to convert files from the brat standoff format to JSONL.

The [our_split_jsonl](our_split_jsonl) directory contains the train, development and test JSONL files that we used for our experiments. We split the [atlantis_dataset.jsonl](atlantis_dataset.jsonl) file into three parts: train, development and test according to a 80:10:10 ratio in terms of overall number of tokens and numbers of entity labels. We also ensured that text covering each geographic area was present in all three parts. Our dataset of 101400 tokens contains 16777 entity labels (which can span one or more tokens) and 3051 relation labels (which connect exactly two entity labels, with a direction). In total, 18030 tokens are annotated with at least one entity label, which corresponds to almost one in five tokens.

## Dataset Composition
|                        | Train nb. | Dev nb. | Test nb. | Total nb. |
|------------------------|-----------|---------|----------|-----------|
| Tokens                 | 83851     | 8156    | 9393     | 101400    |
| Unlabelled tokens      | 69200     | 6507    | 7663     | 83370     |
| Entity-labelled tokens | 14651     | 1649    | 1730     | 18030     |
| Entity labels          | 13582     | 1476    | 1719     | 16777     |
| Relation labels        | 2507      | 222     | 322      | 3051      |

### Entity Label Count
| Label              | Train nb. | Dev nb. | Test nb. | Total nb. |
|--------------------|-----------|---------|----------|-----------|
| geographic feature | 6602      | 692     | 801      | 8095      |
| name               | 3486      | 391     | 462      | 4339      |
| geographic name    | 3494      | 393     | 456      | 4343      |

### Relation Label Count
| Label               | Train nb. | Dev nb. | Test nb. | Total nb. |
|---------------------|-----------|---------|----------|-----------|
| is an element of    | 1300      | 109     | 190      | 1599      |
| is marked by        | 143       | 13      | 17       | 173       |
| is off the coast of | 21        | 1       | 1        | 23        |
| is N of             | 84        | 9       | 8        | 101       |
| is NNE of           | 46        | 1       | 3        | 50        |
| is NE of            | 47        | 1       | 5        | 53        |
| is ENE of           | 72        | 11      | 6        | 89        |
| is E of             | 92        | 6       | 11       | 109       |
| is ESE of           | 73        | 8       | 13       | 94        |
| is SE of            | 42        | 4       | 1        | 47        |
| is SSE of           | 51        | 10      | 3        | 64        |
| is S of             | 84        | 8       | 12       | 104       |
| is SSW of           | 86        | 6       | 7        | 99        |
| is SW of            | 45        | 2       | 5        | 52        |
| is WSW of           | 75        | 10      | 6        | 91        |
| is W of             | 75        | 10      | 11       | 96        |
| is WNW of           | 76        | 4       | 5        | 85        |
| is NW of            | 32        | 2       | 8        | 42        |
| is NNW of           | 63        | 7       | 10       | 80        |