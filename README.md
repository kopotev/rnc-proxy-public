## What Mychat RNC can do

This project provides a natural-language interface from a custom GPT to the Russian National Corpus (RNC) through a Cloudflare Worker.

### Architecture

All MyGPT Action requests should use `openapi-v3.yaml` and go to:

```text
ChatGPT / MyGPT
    ↓
Cloudflare Worker
https://ruscorpora-proxy.mihail-kopotev.workers.dev
    ↓
Russian National Corpus API
https://ruscorpora.ru
```

The Worker decides whether a request should be completed immediately or moved to a background Durable Object job:

- exact counts, IPM, corpus statistics and metadata lookups are compact direct requests;
- small concordance requests are returned directly;
- small MAIN native frequency lists are returned directly;
- large/full concordances are collected in background jobs;
- large frequency jobs are collected through background concordance processing and can be downloaded as CSV.

The default automatic-routing thresholds are configurable in `wrangler.jsonc`:

- `AUTO_DIRECT_EXAMPLE_LIMIT=50`;
- `AUTO_DIRECT_FREQUENCY_LIMIT=500`.

### RNC traffic identification

Every outbound request from the Worker to the RNC is tagged so that RNC developers can distinguish this application from other traffic:

```text
User-Agent: rnc-proxy-chatgpt/3.0.0
X-RNC-Client: rnc-proxy-chatgpt
```

The client identifier can be changed with the `RNC_CLIENT_ID` Worker variable. The dedicated RNC API token remains stored as the Cloudflare secret `RUSCORPORA_TOKEN`.

### Main Corpus (`MAIN`)

- exact hit counts, number of texts, and IPM;
- lexical and grammatical searches;
- distance constraints between words;
- ranked frequency lists by lemma, word form, grammatical tag, or part of speech;
- concordance examples;
- full concordance collection and CSV export;
- word portraits;
- chronology and metadata comparisons via batch exact counts.

### General Internet Corpus of Russian (`GICR`)

- exact counts and IPM;
- lexical and grammatical searches;
- comparisons between several queries;
- subcorpus filtering by supported metadata such as date, author sex, birth year, publication type, and country;
- background concordance collection.

### Example requests

> How often does *Хельсинки* occur in GICR?

> Find the most frequent nouns in the pattern *подозревать … в + noun*.

> Compare *в аэропорту* and *в аэропорте* in the Main Corpus.

> Plot the chronology of *литературный язык* in 25-year periods and report hits, texts and IPM.

> Find verbs of speech in posts written by women from Kazakhstan in GICR.

> Give me all concordance examples and a CSV file.

## Important limitations

- Native RNC ranked frequency tables currently work only for `MAIN`.
- For MAIN native frequency, query settings may differ from exact-count settings. Results produced with different `disambmod` or `distmod` settings must not be treated as the same query.
- Large background frequency tables are concordance-derived rather than the native RNC frequency endpoint. Check job completeness before treating them as exhaustive.
- Full concordance rows are not always identical to corpus occurrences; one snippet may represent more than one occurrence. Job status exposes completeness information when it can be established.
- Concordance and frequency CSV files are generated on demand from temporary Durable Object storage rather than stored permanently.
- Finished job storage is removed after the configured TTL (24 hours by default).
- Hard safety limits currently allow up to 10,000 pages and 2,000,000 collected rows per background job.
- Corpus results depend on the current RNC data and annotation. Counts from different query settings or corpora may differ substantially.
