# TyDi QA - WANA: A Benchmark for Information-Seeking Question Answering in Typologically Diverse Languages of West Asia and North Africa

[**Task**](#the-task) | [**Download**](#download-the-dataset) |
[**Evaluation**](#evaluation)

This repository contains information about TyDi QA - WANA.

## Introduction

TyDi QA - WANA is a question answering dataset covering 10 language varieties
with 52K question-answer pairs. It is an extension of the original TyDi QA dataset; more information can be found [here](https://github.com/google-research-datasets/tydiqa).

To provide a realistic information-seeking task
and avoid priming effects, questions are written by people who want to know the
answer, but don't know the answer yet, (unlike SQuAD and its descendents) and
the data is collected directly in each language without the use of translation
(unlike MLQA and XQuAD).

## The Task

We adopt the **Minimal answer span (MinSpan)** task from the original TyDi QA
dataset. The task description is:

 -  Given the full text of an article, return one of (a) the start and end byte
    indices of the minimal span that completely answers the question; (b) YES or
    NO if the question requires a yes/no answer and we can draw a conclusion
    from the passage; (c) NULL if it is not possible to produce a minimal answer
    for this question.

We emphasize here that the input includes the **full text**
of a Wikipedia article, resulting in significantly more input
text than most multilingual QA datasets include. The table
below lists the average length of articles in our dataset, by
language variety.

| Variety | Characters | Tokens (Whitespace) |
| -------- | ------- | ------- |
| Algerian Arabic | 33.8K | 5.5K |
| Egyptian Arabic | 36.1K | 5.9K |
| Iraqi Arabic | 31.5K | 5.1K |
| Jordanian Arabic | 32.6K | 5.3K |
| Armenian | 48.8K | 6.0K |
| Azerbaijani | 28.3K | 3.6K |
| Farsi | 25.3K | 4.6K |
| Hebrew | 22.7K | 3.8K |
| Tajik | 17.5K | 2.6K |
| Turkish | 17.4K | 2.2K |
| Macro-Avg | 29.4K | 4.5K |

## Download the Dataset

The data can be downloaded using the following links:

 - Train, All Varieties: [download](https://storage.googleapis.com/tydiqa/wana/v1.0/train.all.jsonl)
 - Train, By Variety: [download](https://storage.googleapis.com/tydiqa/wana/v1.0/train.by_variety.jsonl.tar.gz)
 - Dev, All Varieties: [download](https://storage.googleapis.com/tydiqa/wana/v1.0/dev.all.jsonl)
 - Dev, By Variety: [download](https://storage.googleapis.com/tydiqa/wana/v1.0/dev.by_variety.jsonl.tar.gz)
 - Test, All Varieties: [download](https://storage.googleapis.com/tydiqa/wana/v1.0/test.all.jsonl)
 - Test, By Variety: [download](https://storage.googleapis.com/tydiqa/wana/v1.0/test.by_variety.jsonl.tar.gz)

The "All Varieties" versions are single JSONL files containing the entire split.
The "By variety" versions are tarballs containing 10 JSONL files, organized by
language variety.

Each line in the JSONL files is a JSON-encoded dictionary representing a
question and one or more gold answer annotations. The dictionaries have the
following fields:

 - `language`: String. The language variety of the example.
 - `article_plaintext`: String. The text of the Wikipedia article.
 - `question`: String. The question.
 - `answer_types`: List[String]. The answer types for each of this example's
  answer annotations. Possible values are `minimal_span`, `yes_no`, and
  `no_answer`.
 - `answer_start_byte_indices`: List[Number]. Byte indices for the beginning of
  each minimal span answer. `-1` for non-minimal-span answers.
 - `answer_end_byte_indices`: List[Number]. Byte indices for the end of each
  minimal span answer. `-1` for non-minimal-span answers.
 - `answer_texts`: List[String]. A text representation of each answer. For
  minimal span answers, it is the result of slicing the byte encoding of the
  article plaintext using the corresponding indices. For yes/no answers, is is
  either "YES" or "NO". When no answer was found, it is "NULL".

## Evaluation

To evaluate using our metrics (F1 and Exact Match), use the
[WANA_Metrics.ipynb](WANA_Metrics.ipynb) notebook. To use it you must first
prepare your model's outputs on your chosen split. The simplest way to do
so is to create a version of the JSONL file for you chosen split that contains
1-3 additional fields (names are configurable in the notebook):

 - `generated_answer`: String. The model's answer. Should be either "YES", "NO", "no answer", or a substring of the article.
 - `generated_answer_byte_start_index`: Optional String. For minimal span answers, the start byte index of the model's prediction; -1 for other answers.
 - `generated_answer_byte_end_index`: Optional String. As above, but the end byte index.

Providing start and end indices for predicted answers is optional. If your model
just generates the text of the answer, they are not required, and will be
inferred based on the first occurence of the generated answer within the article
text. If your model instead predicts indices, you should provide these fields.
Note that the `generated_answer` field is still required, and for minimal span
answers, it should be calculated from the provided start/end byte indices.

## Source Data

The articles for TyDi QA are drawn from single coherent snapshots of Wikipedia
from February 1st 2023. Unfortunately, these snapshots appear to no longer be
available online. The most recent snapshots can be downloaded from the following
URL template:
`https://dumps.wikimedia.org/${LANG}wiki/latest/${LANG}wiki-latest-pages-articles-multistream.xml.bz2`,
where `LANG` is in `[ar, az, fa, he, hy, tg, tr]`.

## Contact us

If you have a technical question regarding the dataset, code or publication,
please create an issue in this repository. This is the fastest way to reach us.

## Disclaimer

This is not an officially supported Google product.
