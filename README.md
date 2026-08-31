## What Mychat RNC can do
This chat provides a natural-language interface to the Russian National Corpus (RNC). **You need an active subscription to ChatGPT to access the chat.**
You can describe a corpus query in your own language. At the moment, the chat supports two corpora:

* **Main Corpus (`MAIN`)**

  * exact hit counts, number of texts, and IPM;
  * lexical and grammatical searches;
  * distance constraints between words;
  * ranked frequency lists by **lemma, word form, grammatical tag, or part of speech**;
  * concordance examples;
  * full concordance collection and CSV export;
  * word portraits.

* **General Internet Corpus of Russian (`GICR`)**

  * exact counts and IPM;
  * lexical and grammatical searches;
  * comparisons between several queries;
  * subcorpus filtering by supported metadata such as date, author sex, birth year, publication type, and country.

### Example requests

You can ask things such as:

> How often does *Хельсинки* occur in GICR?

> Find the most frequent nouns in the pattern *подозревать … в + noun*.

> Compare *в аэропорту* and *в аэропорте* in the Main Corpus.

> Find verbs of speech in the posts written by women from Kazakhstan in GICR.

> Give me all concordance examples and a CSV file.

## Important limitations
* **The chat works for users with an active paid subscription to ChatGPT. The developer has no responsibility for any additional costs.**
* **Native ranked frequency tables currently work only for `MAIN`.**
* In the Main Corpus, native frequency tables effectively use:
  * `disambmod=main`
  * `distmod=no_zeros`
* Exact counts can use other settings when explicitly requested. Results obtained with different `disambmod` or `distmod` settings should not be compared as if they were the same query.
* The chat does **not currently expose other RNC subcorpora**. Support for those can be added later.
* Full concordance rows are not identical to corpus occurrences; one snippet may represent more than one occurrence.
* Concordance CSV files are generated on demand from temporary Durable Object storage rather than stored as permanent files.
* Finished job storage is removed after the configured TTL (24 hours by default).
* Very large responses may hit technical size limits. In such cases, the chat may reduce the displayed sample or use a background concordance job.
* Corpus results depend on the current RNC data and annotation. Counts from different query settings or corpora may differ substantially.
