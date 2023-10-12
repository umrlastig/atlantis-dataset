# ATLANTIS Dataset
This repository contains the ATLANTIS (coAsTaL mAritime NavigaTion InstructionS) dataset files and our benchmark results on this dataset.

This work is co-financed by the Shom and the IGN and is being carried out at the LASTIG, a research unit at Université Gustave Eiffel.

Under no circumstances should any part of this work be used for real-life navigation.

## Overview
ATLANTIS is a French-language dataset for nested spatial entity and binary spatial relation extraction from text. The dataset is composed of extracts from 15 different volumes of the _Instructions nautiques_. The _Instructions nautiques_ are a series of books produced and published by the French Naval Hydrographic and Oceanographic Service, the [Shom](https://www.shom.fr/). Each volume contains essential information for navigating safely in the coastal waters of a specific geographic area, including instructions for entering ports and descriptions of the coastal maritime environment. We extracted the text from the PDF documents using [pdfminer.six](https://github.com/pdfminer/pdfminer.six). The volumes, which are written in French, cover coastal areas in Africa, Europe, North and South America, as well as in the Indian and Pacific Oceans.

The TextMine'24 Dataset is a subset of the ATLANTIS Dataset and contains nested spatial entity annotations. It was used as the benchmark dataset for the [_Défi TextMine 2024_](https://www.kaggle.com/competitions/defi-textmine-2024), a spatial entity recognition challenge hosted by the [TextMine working group](https://textmine.sciencesconf.org/) which is part of [EGC](https://www.egc.asso.fr/), the International Francophone Association for Knowledge Extraction and Management.

## Annotation Scheme
We annotated our dataset by hand using the [brat rapid annotation tool](http://brat.nlplab.org), which allows creating nested labelled annotations and creating directed labelled links between them. The source text was split on whitespace by brat, giving a dataset of 101,400 tokens.

### Nested Spatial Entities
We use three labels to annotate unnamed and named nested spatial entities:
- **geographic feature** refers to common nouns that represent types of spatial entities
- **name** refers to pure proper nouns
- **geographic name** refers to the full name associated with a geographic feature

Any token can be annotated with zero or one **geographic feature** or **name** label. A token cannot be annotated with both a **geographic feature** and a **name** label. A token cannot be annotated only with a **name** label. A token annotated with a **name** label must also be annotated one or more times with a **geographic name** label. Any token, already labelled or not, can be annotated zero or more times with a **geographic name** label.

### Binary Spatial Relations
We use 19 labels to annotate binary spatial relations:
- **is off the coast of** refers to an entity that is off the coast of another (it is always referred to using the same words in French in our dataset: "au large de")
- **is marked by** refers to an entity that is marked by another (usually a navigation mark such as a buoy or a beacon)
- **is an element of** refers to an entity that is situated on or in another such that a bird's eye view shows the spatial footprint of one as being within or partly within the other
- **is [direction] of** refers to an entity that is in any one of 16 cardinal directions with respect to another: the 16 relations include four that use the cardinal directions (N, E, S, W), four that use the intercardinal directions (NE, SE, SW, NW) and eight that use the secondary intercardinal directions (NNE, ENE, ESE, SSE, SSW, WSW, NWN, NNW)

All relation annotations must link two entity annotations with the exception of **name** entity labels, which cannot be part of relation annotations. All relation annotations must have a direction.

## Repository Content
The [atlantis_dataset_brat](atlantis_dataset_brat) directory contains the .txt source files and the corresponding .ann annotation files for the entire dataset.

The [atlantis_dataset.jsonl](atlantis_dataset.jsonl) file contains the entire annotated dataset in a JSONL format. The specific JSONL format is that of the [SCIERC dataset](http://nlp.cs.washington.edu/sciIE/). We used [this script](https://github.com/dwadden/dygiepp/blob/master/scripts/new-dataset/brat_to_input.py), modified to use the [fr_core_news_sm](https://spacy.io/models/fr#fr_core_news_sm) spaCy model for tokenisation, to convert files from the brat standoff format to JSONL.

The [our_split_jsonl](our_split_jsonl) directory contains the train, development and test JSONL files that we used for our experiments. We split the [atlantis_dataset.jsonl](atlantis_dataset.jsonl) file into three parts: train, development and test according to a 80:10:10 ratio in terms of overall number of tokens and numbers of entity labels. We also ensured that text covering each geographic area was present in all three parts. Our dataset of 101,400 tokens contains 16,777 entity labels (which can span one or more tokens) and 3,051 relation labels (which connect exactly two entity labels, with a direction). In total, 18,030 tokens are annotated with at least one entity label, which corresponds to almost one in five tokens.

## Dataset Composition
|| Train nb. | Dev nb. | Test nb. | Total nb. |
|------------------------|-----------|---------|----------|-----------|
| Tokens | 83,851    | 8,156   | 9,393    | 101,400   |
| Unlabelled tokens      | 69,200    | 6,507   | 7,663    | 83,370    |
| Entity-labelled tokens | 14,651    | 1,649   | 1,730    | 18,030    |
| Entity labels  | 13,582    | 1,476   | 1,719    | 16,777    |
| Relation labels| 2,507     | 222     | 322      | 3,051     |

### Entity Label Count
| Label      | Train nb. | Dev nb. | Test nb. | Total nb. |
|--------------------|-----------|---------|----------|-----------|
| geographic feature | 6,602     | 692     | 801      | 8,095     |
| name       | 3,486     | 391     | 462      | 4,339     |
| geographic name    | 3,494     | 393     | 456      | 4,343     |

### Relation Label Count
| Label       | Train nb. | Dev nb. | Test nb. | Total nb. |
|---------------------|-----------|---------|----------|-----------|
| is an element of    | 1,300     | 109     | 190      | 1,599     |
| is marked by| 143       | 13      | 17       | 173       |
| is off the coast of | 21| 1       | 1| 23|
| is N of     | 84| 9       | 8| 101       |
| is NNE of   | 46| 1       | 3| 50|
| is NE of    | 47| 1       | 5| 53|
| is ENE of   | 72| 11      | 6| 89|
| is E of     | 92| 6       | 11       | 109       |
| is ESE of   | 73| 8       | 13       | 94|
| is SE of    | 42| 4       | 1| 47|
| is SSE of   | 51| 10      | 3| 64|
| is S of     | 84| 8       | 12       | 104       |
| is SSW of   | 86| 6       | 7| 99|
| is SW of    | 45| 2       | 5| 52|
| is WSW of   | 75| 10      | 6| 91|
| is W of     | 75| 10      | 11       | 96|
| is WNW of   | 76| 4       | 5| 85|
| is NW of    | 32| 2       | 8| 42|
| is NNW of   | 63| 7       | 10       | 80|

## Benchmark Results
We provide benchmark results for this dataset for three tasks: nested spatial entity extraction, binary spatial relation extraction, and end-to-end nested spatial entity and binary spatial relation extraction.

Our benchmark results were obtained using a supervised deep learning approach for automatic nested spatial entity and binary spatial relation extraction from text that we presented at the [7th ACM SIGSPATIAL International Workshop on Geospatial Humanities](https://ludovicmoncla.github.io/sigspatial-geohumanities-2023/index.html), held in conjunction with [ACM SIGSPATIAL 2023](http://sigspatial2023.sigspatial.org/). Our approach involves applying [PURE](https://github.com/princeton-nlp/PURE) (the Princeton University Relation Extraction system), made for _flat_, _generic_ entity extraction and _generic_ binary relation extraction, to the extraction of _nested_, _spatial_ entities and _spatial_ binary relations. The advantage of extracting _nested_ spatial entities and the _spatial_ relations between them is that it captures more information that can aid entity disambiguation.

This work was granted access to the HPC resources of IDRIS under the allocation 2022-AD011013699 made by GENCI.

### Hyperparameters
| Hyperparameter     | Entity Model | Relation Model |
|--------------------|--------------|----------------|
| Learning rate      | 1e-5         | 2e-5           |
| Task learning rate | 5e-4         | -              |
| Train batch size   | 16           | 32             |
| Training epochs    | 100          | 10             |
### Results
#### F1-Scores for Varying Context Window Sizes
Mean micro F1-score with standard deviation over five runs for varying context window ($W$) sizes for: entity extraction, relation extraction from gold entity annotations, and end-to-end entity and relation extraction (e2e) from best predicted entity annotations ($W$ = 248 for monolingual, $W$ = 200 for multilingual). For each task, the highest F1-score over all context window sizes for each base encoder is in **bold**, and the overall highest F1-score over all context window sizes and both base encoders is indicated with an asterisk (*).

| Context Window | Entity / Mono. | Entity / Multi. | Relation / Mono. | Relation / Multi. | e2e / Mono.     | e2e / Multi.   |
|----------------|----------------|-----------------|------------------|-------------------|-----------------|----------------|
| $W$=0          | 91.1 ± 0.3     | 91.9 ± 0.2      | **64.2*** ± 2.2  | **63.2** ± 1.0    | **63.9*** ± 2.2 | **63.2** ± 1.2 |
| $W$=50         | 92.1 ± 0.2     | 92.3 ± 0.3      | 64.2 ± 1.4       | 63.0 ± 1.7        | 63.8 ± 1.4      | 63.1 ± 1.7     |
| $W$=100        | 91.9 ± 0.2     | 92.3 ± 0.2      | 63.7 ± 0.7       | 62.9 ± 0.7        | 63.6 ± 0.7      | 62.9 ± 0.8     |
| $W$=150        | 91.9 ± 0.2     | 92.2 ± 0.2      | -                | -                 | -               | -              |
| $W$=200        | 92.0 ± 0.2     | **92.3*** ± 0.2 | -                | -                 | -               | -              |
| $W$=248        | **92.2** ± 0.2 | 92.3 ± 0.2      | -                | -                 | -               | -              |

#### Detailed Nested Entity Extraction Results
\[Prec.|Rec.|F1\] / \[Mono.|Multi\] gives the mean [precision|recall|micro F1-score] for the [monolingual|multilingual] model over five runs for entity extraction for each entity label using the context window that gives the best overall results ($W$ = 248 for monolingual, $W$ = 200 for multilingual). For each task, the highest precision, recall and micro F1-score for all entity labels and for all relation labels over both base encoders is in **bold**.

| Label                 | Prec. / Mono. | Prec. / Multi. | Rec. / Mono. | Rec. / Multi. | F1 / Mono. | F1 / Multi. |
|-----------------------|---------------|----------------|--------------|---------------|------------|-------------|
| _all entity labels_   | 94.6          | **95.2**       | **89.8**     | 89.6          | 92.2       | **92.3**    |
| geographic feature    | 94.1          | 94.4           | 95.8         | 95.1          | 95.0       | 94.8        |
| name                  | 97.7          | 97.4           | 78.0         | 78.4          | 86.7       | 86.9        |
| geographic name       | 92.9          | 94.9           | 91.3         | 91.4          | 92.1       | 93.1        |
#### Detailed Binary Relation Extraction Results
\[Prec.|Rec.|F1\] / \[Mono.|Multi\] gives the mean [precision|recall|micro F1-score] for the [monolingual|multilingual] model over five runs for relation extraction for each relation label from gold entity annotations using the context window that gives the best overall results ($W$ = 0 for monolingual and for multilingual). For each task, the highest precision, recall and micro F1-score for all entity labels and for all relation labels over both base encoders is in **bold**.

| Label                 | Prec. / Mono. | Prec. / Multi. | Rec. / Mono. | Rec. / Multi. | F1 / Mono. | F1 / Multi. |
|-----------------------|---------------|----------------|--------------|---------------|------------|-------------|
| _all relation labels_ | **70.8**      | 67.2           | 58.8         | **59.9**      | **64.2**   | 63.2        |
| is an element of      | 70.5          | 67.9           | 60.2         | 60.3          | 64.9       | 63.8        |
| is marked by          | 64.9          | 55.1           | 51.8         | 49.4          | 57.5       | 50.8        |
| is off the coast of   | 100.0         | 100.0          | 100.0        | 100.0         | 100.0      | 100.0       |
| is N of               | 48.6          | 39.8           | 52.5         | 50.0          | 49.9       | 44.2        |
| is NNE of             | 76.7          | 37.3           | 73.3         | 53.3          | 74.1       | 43.3        |
| is NE of              | 95.0          | 100.0          | 64.0         | 60.0          | 76.1       | 75.0        |
| is ENE of             | 83.0          | 93.3           | 63.3         | 83.3          | 71.6       | 87.9        |
| is E of               | 62.5          | 57.1           | 45.5         | 41.8          | 52.6       | 48.1        |
| is ESE of             | 73.4          | 71.1           | 55.4         | 66.2          | 62.6       | 67.8        |
| is SE of              | 90.0          | 60.0           | 100.0        | 100.0         | 93.3       | 73.3        |
| is SSE of             | 73.7          | 81.3           | 73.3         | 66.7          | 68.8       | 69.3        |
| is S of               | 84.7          | 82.1           | 71.7         | 81.7          | 77.3       | 81.1        |
| is SSW of             | 87.4          | 74.1           | 68.6         | 85.7          | 76.0       | 79.3        |
| is SW of              | 0.0           | 0.0            | 0.0          | 0.0           | 0.0        | 0.0         |
| is WSW of             | 92.0          | 83.6           | 66.7         | 70.0          | 77.1       | 75.3        |
| is W of               | 79.3          | 80.4           | 65.5         | 60.0          | 71.3       | 68.0        |
| is WNW of             | 100.0         | 89.3           | 88.0         | 88.0          | 93.3       | 87.9        |
| is NW of              | 67.3          | 87.4           | 35.0         | 50.0          | 45.7       | 63.0        |
| is NNW of             | 61.7          | 64.1           | 44.0         | 40.0          | 51.1       | 49.0        |
#### Detailed End-to-End Entity and Relation Extraction Results
\[Prec.|Rec.|F1\] / \[Mono.|Multi\] gives the mean \[precision|recall|micro F1-score\] for the \[monolingual|multilingual\] model over five runs for end-to-end entity and relation extraction for each relation label from the best predicted entity annotations using the context window that gives the best overall results ($W$ = 0 for monolingual and for multilingual). The highest precision, recall and F1-score for all relation labels over both base encoders is in **bold**.

| Label                 | Prec. / Mono. | Prec. / Multi. | Rec. / Mono. | Rec. / Multi. | F1 / Mono. | F1 / Multi. |
|-----------------------|---------------------|----------------------|--------------------|---------------------|------------------|-------------------|
| _all relation labels_ | **70.2**            | 67.3                 | 58.8               | **59.8**            | **63.9**             | 63.2              |
| is an element of      | 70.2                | 68.2                 | 60.2               | 60.2                | 64.8             | 63.9              |
| is marked by          | 61.3                | 55.1                 | 51.8               | 49.4                | 56.1             | 50.8              |
| is off the coast of   | 100.0               | 100.0                | 100.0              | 100.0               | 100.0            | 100.0             |
| is N of               | 47.7                | 41.8                 | 52.5               | 50.0                | 49.7             | 45.2              |
| is NNE of             | 76.7                | 39.3                 | 73.3               | 53.3                | 74.1             | 44.8              |
| is NE of              | 95.0                | 100.0                | 64.0               | 60.0                | 76.1             | 75.0              |
| is ENE of             | 96.0                | 93.3                 | 63.3               | 83.3                | 75.9             | 87.9              |
| is E of               | 67.9                | 57.1                 | 45.5               | 41.8                | 54.4             | 48.1              |
| is ESE of             | 70.9                | 71.1                 | 55.4               | 66.2                | 61.6             | 67.8              |
| is SE of              | 90.0                | 60.0                 | 100.0              | 100.0               | 93.3             | 73.3              |
| is SSE of             | 71.7                | 81.3                 | 73.3               | 66.7                | 67.1             | 69.3              |
| is S of               | 84.7                | 82.1                 | 71.7               | 81.7                | 77.3             | 81.1              |
| is SSW of             | 87.4                | 74.1                 | 68.6               | 85.7                | 76.0             | 79.3              |
| is SW of              | 0.0                 | 0.0                  | 0.0                | 0.0                 | 0.0              | 0.0               |
| is WSW of             | 92.0                | 83.6                 | 66.7               | 70.0                | 77.1             | 75.3              |
| is W of               | 75.8                | 78.9                 | 65.5               | 60.0                | 69.5             | 67.3              |
| is WNW of             | 100.0               | 89.3                 | 88.0               | 88.0                | 93.3             | 87.9              |
| is NW of              | 80.0                | 87.4                 | 35.0               | 50.0                | 47.0             | 63.0              |
| is NNW of             | 63.5                | 64.1                 | 44.0               | 40.0                | 51.8             | 49.0              |
