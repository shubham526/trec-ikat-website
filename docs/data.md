# **Datasets and Resources**
---

## :boom: **Topics for iKAT Year 2 (2024)**

|    File      |      Description      |
|:------------|:---------------------|
| 	Release later       |       Train topics in JSON format.             |

## :boom: **Topics for iKAT Year 1 (2023)**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2023_train_topics.json](https://drive.google.com/file/d/1sNHmVYO9PVG2kFxLscPGhN-uCCUuDAu9/view?usp=sharing)       |       Train topics in JSON format.             |
| 	[2023_test_topics.json](https://drive.google.com/file/d/1zPSiAqLmbx9QFGm6walnuMUl7xoJmRB7/view?usp=sharing)       |       Test topics in JSON format.             |

## :boom: **Addtional Data**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2023_train_topics_psg_text.jsonl](https://drive.google.com/file/d/1Bk90f0Rd982Px65GDuQayd5s8uXQs9UX/view?usp=sharing)       |       Text of provenance passages in the train topics.             |
| 	[2023_test_topics_psg_text.jsonl](https://drive.google.com/file/d/1YGhJAUEw9PPITPkrWP3EtnUsWiboh3eq/view?usp=sharing)       |       Text of provenance passages in the test topics.             |
| 	[2023_passages_hashes.tsv.bz2](https://ikattrecweb.grill.science/ikat_2023_passages_hashes.tsv.bz2)       |  TSV file containing MD5 hashes of passage texts. The `.tsv` file has this format: `doc_id   passage_number    passage_MD5`. Total download size is 2.2GB.           |
| 	[2023_top_1000_query_results.zip](https://drive.google.com/file/d/1csWaKo0WxZVACrMazlJLcW2Xy1msp9ec/view?usp=sharing)       |       This zip file has queries from both training and testing topics, saved in `queries_train.txt` and `queries_test.txt` respectively. The results from the [iKAT searcher](https://ikat-searcher.grill.science/) (`BM25` using manually resolved queries) are saved in the `query_results_train` and `query_results_test` folders. Each result file, with up to 1000 results, corresponds to a query based on line numbers, starting from zero. For instance, the results for the first query in `queries_train.txt` can be found in `query_results_train/query_results_000.txt`. In each result file, every line shows the `ClueWeb22 ID` followed by the `URL`.            |

## :boom: **Baseline Runs (iKAT 2023)**

Below, we provide two baseline runs. 

- **Method.**
	- `BM25+RM3` (Pyserini default) as the initial retrieval (denoted by `ret_bm25_rm3` in the file name) method to retrieve 100 passages per query (denoted by `k_100`).
	- The query in each turn was re-written using: 
		1. The context, and 
		2. The top-3 relevant PTKB statements (denoted by `num_ptkb_3` in the file name).
- **Query re-writing.** In all cases, the re-written query was construted by appending the relevant PTKB statements to the (manually or automatically) resolved query.
- **Response generation.** A response was generated using the top-3 passages retrieved with the re-written query (denoted by `num_psg_3` in the file name). We use the `T5` model [`mrm8488/t5-base-finetuned-summarize-news`](https://huggingface.co/mrm8488/t5-base-finetuned-summarize-news) available on HuggingFace for this purpose. 
- **For automatic runs.** 
	- The relevant PTKB statements were determined automatically by re-ranking the statements using `SentenceTransformers`, specifically, the model [`cross-encoder/ms-marco-MiniLM-L-6-v2`](https://huggingface.co/cross-encoder/ms-marco-MiniLM-L-6-v2) available on HuggingFace.
	- The query was re-written automatically using the [`castorini/t5-base-canard`](https://huggingface.co/castorini/t5-base-canard) model available on HuggingFace.
- **For manual runs.** 
	- The relevant PTKB statements provided in the `ptkb_provenance` field were used. 
	- The manually re-written query provided in the `resolved_utterance` field was used.



|    File      |      Run Type     |
|:------------|:---------------------|
| 	[ret_bm25_rm3--type_automatic--num_ptkb_3--k_100--num_psg_3.official.run.json](https://drive.google.com/file/d/14MyYzEYI7D5rBIsRjYfokyMU-9MQwm6t/view?usp=sharing)       |       Automatic     |
| 	[ret_bm25_rm3--type_manual--num_ptkb_3--k_100--num_psg_3.official.run.json](https://drive.google.com/file/d/1fCjGmULmYoWimr5cgbN_suQ3hAN8YVNs/view?usp=sharing)       |      Manual             |


## :boom: **Document Collection: TREC iKAT 2023/2024 ClueWeb22-B**

The collection distribution is being handled directly by CMU and not the iKAT organizers. Please follow these steps to get your data license ASAP:

- Sign the license form available on the [ClueWeb22 project web page](https://lemurproject.org/clueweb22/obtain.php).
- Send the form to CMU for approval (**jlm4@andrew.cmu.edu**)

Please give enough time to the CMU licensing office to accept your request. A download link will be sent to you by the ClueWeb22 team at CMU.

**Note.** 

- CMU requires a signature from the organization (i.e., the university or company), not an individual who wants to use the data. This can slow down the process at your end too. So, it’s useful to start the process ASAP.
- If you already have an accepted license for ClueWeb22, you don’t need a new form. Please let us know if that’s the case.


## :boom: **Additional Resources**

We provide the following additional resources for the teams:

### **TREC iKAT ClueWeb22-B Passage Collection**

We provide a segmented version of the TREC iKAT 2023 ClueWeb22-B Document collection available from CMU in two formats: `JSONL` and `TrecWeb`. 

In case you have segmented the document collection yourself, you may check whether your segments match ours using the `tsv` file of passage hashes provided. 

- **Passage collection in `JSONL` format.**
	- These files contain passages in the form `{"id": "[passage id]", "contents": "[passage text]", "url": "[ClueWeb22 document URL]"}`
	- Passage IDs are structured as: `doc_id:passage_number`
	- Total download size is approximately 31 GB.
	- Total number of passages is 116,838,987
- **Passage collection in `TrecWeb` format.**
	- Format as shown on website.
	- Total download size is approximately 31 GB
	
- **Passage hashes.**
	- `tsv` file containing MD5 hashes of passage texts.
	- The `.tsv` file has this format: `doc_id   passage_number    passage_MD5`
	- Total download size is 2.2 GB
	
	
### **Sparse Lucene Passage Index**

We also provide a sparse Lucene index generated from the `JSONL` passage files above using Pyserini. The files form a single `.tar.bz2` archive split into sections for simpler downloading due to the overall size. To extract the archive, once downloaded, you must combine each of the sections in name order back into a single file: 

```bash
cat ikat_2023_passage_index.tar.bz2.part* > ikat_203_passage_index.tar.bz2
```

Total download size is approximately 150 GB
	
### **How do I access these resources?** 

Each team should use a URL of `https://ikattrecweb.grill.science/<team_name>` to access the files. The page will ask for a userID and password. Enter the login details which you obtained from the iKAT organizers. You should see a page which lists each type of data and has links to the individual files listed above, along with their checksum files.

**NOTE:** 

- Currently, these teams can access this URL: `MLIA`,  `TREMA_UNH` and `Nota` . Please send us a message privately via slack or through the email and we will share the login details with you.
- Other teams: You have shared IPs in the `10.x.x.x` range which is for private networks, so we need another IP from you. Can you please share another suitable IP with us so that we may configure the above download link to work for you?

### **iKAT Searcher**

iKAT Searcher is a simple tool developed to help with creating the topics for iKAT. The tool allows topic developers to visually assess the behaviour of a retrieval system, ultimately making it easier to develop challenging, but interesting, topics for the Track. You can interact with the system [here](https://ikat-searcher.grill.science/). See the [GitHub repository](https://github.com/andrewramsay/Interactive-CAsT/tree/deployment_testing).

### **Run Validation**

We provide code for run validation in our [Github repository](https://github.com/irlabamsterdam/iKAT/tree/main/2023/scripts/run_validation). Please see the associated README file for detailed instructions on how to run the code. It is crucial to validate your submission files before submitting them. The run files that fail the validation phase will be discarded. We advise you to get familiarized with the validation script as soon as possible and let us know if you have any questions or encounter any problems working with it. 

**Note.** You need the MD5 hash file of the passages in the collection to run the validation code. You can download this file from above. 

Below is a summary of the checks that the script performs on a run file.

 * Can the file be parsed as valid JSON?
 * Can the JSON be parsed into the protocol buffer format defined in `protocol_buffers/run.proto`?
 * Does the run file have at least one turn? 
 * Is the `run_name` field non-empty?
 * Is the `run_type` field non-empty and set to `automatic` or `manual`?
 * Does the number of turns in the run match the number in the test topics file?
 * Does the number of turns in each topic in the run match the corresponding number in the test topics file?
 * For each turn in the run:
   * Is the turn ID valid and matches an entry in the topics file?
   * Is any turn ID higher than expected for the selected topic (e.g. turns 1-5 in the topic, but a turn has ID 6 in the run file)?
   * (optional, enabled by default) Do all the `passage_provenance` passage IDs appear in the collection?
   * For each response in the turn:
     * Does it have a rank > 0?
     * Do the ranks increase with successive responses?
     * Does the response have a non-empty `text` field?
     * For each `passage_provenance` entry:
       * Does it have a score less than the previous entry?
       * Does it have a passage ID containing a single colon and beginning with 'clueweb22-'?
     * Are there less than 1000 `passage_provenance` entries listed for the response?
     * Is there at least one `passage_provenance` with its `used` field set to True in the response?
     * Does the response have at least one `passage_provenance` entry?
     * Does the response have at least one `ptkb_provenance` entry?
     * For each `ptkb_provenance` entry:
       * Does it have a non-empty ID?
       * Does the ID appear in the `ptkb` field of the topic data?
       * Does the text given match that in the topic data?
       * Does it have a score less than the previous entry?




## **Additional Data from TREC CAsT**
We provide the data from previous years' TREC CAsT below. The iKAT topics are similar, with the addition of the Personal Text Knowledge Base. For more information on TREC CAsT, [see the website](https://www.treccast.ai/) and read the overview papers [[2019]](https://arxiv.org/abs/2003.13624) [[2020]](https://scholar.google.com/scholar_url?url=https://www.cs.cmu.edu/afs/cs.cmu.edu/Web/People/callan/Papers/trec2021-dalton.pdf&hl=en&sa=T&oi=gsb-gga&ct=res&cd=0&d=4712262702816537608&ei=En6IZNaeLcPFmAGMo47oDQ&scisig=AGlGAw9ggvQnzye9tQlrvoob1Js5) [[2021]](https://www.cs.cmu.edu/~callan/Papers/trec22-Jeffrey_Dalton.pdf) [[2022]](https://trec.nist.gov/pubs/trec31/papers/Overview_cast.pdf)

**Note.** TREC CAsT did not include a PTKB but you can be creative and modify the data according to your needs. Also, TREC CAsT used different collections (Wikipedia, KILT, MS MARCO, etc.) at different stages. **_iKAT is using a subset of the recently released ClueWeb22-B_**. 

### **CAsT Year 4 (2022)**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2022_automatic_evaluation_topics_tree_v1.0.json](https://drive.google.com/file/d/1eASuL62byYlS9LoJFzKWYMGYQsMz3X6j/view?usp=sharing)       |       Contains each conversation tree (topic) with an automatic rewrite generated for each user utterance.                |
|        [2022_evaluation_topics_turn_ids.json](https://drive.google.com/file/d/1SSLhJ_4GHbnrUuxzt_Vh5kegGTFu4JTA/view?usp=sharing)      |  Contains each conversation tree (topic) with the resolved query for each user utterance.   |
|        [2022_evaluation_topics_tree_v1.0.json](https://drive.google.com/file/d/1f4uD6KGLX8wf_S5yzxOVbkfFuJxA04eU/view?usp=sharing)     |  Contains all ids that responses/ranked passages need to be returned for. |
|        [2022_evaluation_topics_flattened_duplicated_v1.0.json](https://drive.google.com/file/d/1Kenj3U21k1FjmpCrNvfz5XGck11tHbsK/view?usp=sharing)      |             Contains all possible conversation paths across all the conversation trees.       |

### **CAsT Year 3 (2021)**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2021_automatic_evaluation_topics_v1.0.json](https://drive.google.com/file/d/1Fg-xvKeIZx9ly2uFczNEE_UYNozkfLgt/view?usp=sharing)       |       25 primary evaluation topics in JSON format. Variant: Automatic              |
|        [2021_manual_evaluation_topics_v1.0.json](https://drive.google.com/file/d/1vKwxhd7aNJ7nHC5_AphZHVYUvyZRUiQe/view?usp=sharing)      |  25 primary evaluation topics in JSON format. Variant: Manual  |
|        [2021qrels.txt](https://drive.google.com/file/d/1Co9o0xjzEqzosKcuLBrcNl39Q-11flA6/view?usp=drive_link)      |  Qrels file for passage ranking task. |


### **CAsT Year 2 (2020)**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2020_automatic_evaluation_topics_v1.0.json](https://drive.google.com/file/d/1P9nix1pV_kCO2dgunw_KsLMMkWa8b2uy/view?usp=sharing)       |       25 primary evaluation topics in JSON format. Variant: Automatic              |
|        [2020_manual_evaluation_topics_v1.0.json](https://drive.google.com/file/d/1HlJXyicROEcbpAxpPcs1NC1YBrbVcibt/view?usp=sharing)      |  25 primary evaluation topics in JSON format. Variant: Manual  |
|        [2020qrels.txt](https://drive.google.com/file/d/1Wj8gWNZLbYWTvh-3EvVtIn4HFl13Kega/view?usp=sharing)      |  Qrels file for passage ranking task. |

### **CAsT Year 1 (2019)**

|    File      |      Description      |
|:------------|:---------------------|
| 	[train_topics_v1.0.json](https://drive.google.com/file/d/1xQTcGtyDMptoXYmapjCwI6Othv91ebdW/view?usp=sharing)       |      30 example training topics in JSON format.           |
|        [evaluation_topics_v1.0.json](https://drive.google.com/file/d/17beCNDgSyDODJAFmvfi7-T1CmRmXm2MX/view?usp=sharing)      |  50 evaluation topics in JSON format.  |
|        [2019qrels.txt](https://drive.google.com/file/d/1qaFqUISt_JD_y6CIUfIa7aDWTX38Y93p/view?usp=sharing)      |  Official evaluation qrels file for passage ranking task. |
|        [train_qrels.txt](https://drive.google.com/file/d/1VgpSigtZHKcFKmgaowHaXKXQTvZz8QrV/view?usp=sharing)      |   Limited (incomplete) training judegements for 5 topics (approximately 50 turns). The judgments are graded on a three point scale (2 very relevant, 1 relevant, and 0 not relevant). |

