# OS_in_BiH
Codebase and datasets used in "Politics, Identity, and Ontological Security in Bosnia and Herzegovina." All notebooks were run in Google Colab using T4 runtimes and only modified to provide clearer interpretation of the code. 

## Purpose
This analysis was designed to determine how political discourse in the Parliamentary Assembly of Bosnia and Herzegovina affects the state's ontological security. This analysis uses modern techniques based largely on the transformers architecture and provides a macro analysis of discourse among the political elite in BiH.

## Data
The orginial dataset can be found at [GESIS](https://search.gesis.org/research_data/SDN-10.7802-2387). It is a corpus of Parliamentary debate in BiH between 1998-2018. A subset of data from 2010-2018 was selected for this analysis.

The data was converted from RDS using RStudio and converted to CSV for use in Python.

```R

library(tidyverse)
df <- readRDS("BiH_02-07_term_final.RDS") %>%
	mutate(date = ymd(date),
		   date = str_replace_all(date, "3013-", "2013-"),
		   date_raw = str_replace_all(date_raw, ".3013", ".2013")) %>%
	filter(date >= ymd("2010-01-01")) %>%
	write_csv("parliment_debates_BA.csv")

```

## Preprocessing

The data was preprocessed using the [preprocessing script](/PreProcessing.ipynb). The preprocessing includes machine translation using [opus-mt-sla-en](https://huggingface.co/Helsinki-NLP/opus-mt-sla-en), an initial filter for procedural speeches using [ParlaCAP-Topic-Classifier](https://huggingface.co/classla/ParlaCAP-Topic-Classifier), and chunked into 512-token segments using RecursiveCharacterTextSplitter.

## Model Training

A custom multilabel classifier was trained for this analysis, and the detail can be found in the [model training notebook](/ModelTraining) and on [HuggingFace](https://huggingface.co/zastuck/roberta-base-bosnian-parliament-multilabel-v1). The final model can also be found on [HuggingFace](https://huggingface.co/zastuck/roberta-base-bosnian-parliament-multilabel-v1) along with the [final training dataset](https://huggingface.co/datasets/zastuck/bosnian-parliament-700)

## Machine Aided Techniques

This analysis combined multilabel classification, sentiment analysis, and topic analysis using maching aided techniques. The multilabel calssifier is outlined in the section above, the sentiment was calculated with [xlm-r-parlasent](https://huggingface.co/classla/xlm-r-parlasent), and the topic analysis was done using [BERTopic](https://maartengr.github.io/BERTopic/index.html). All procedures and parameters are outlined in the [machine assisted techniques script](/MachineAssistedTechniques.ipynb)


