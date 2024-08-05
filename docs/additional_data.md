# **Additional Data**
---

## **iKAT Year 1 (2023)**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2023_train_topics.json](https://drive.google.com/file/d/1sNHmVYO9PVG2kFxLscPGhN-uCCUuDAu9/view?usp=sharing)       |       Train topics in JSON format.             |
| 	[2023_test_topics.json](https://drive.google.com/file/d/1zPSiAqLmbx9QFGm6walnuMUl7xoJmRB7/view?usp=sharing)       |       Test topics in JSON format.             |
| 	[2023_train_topics_psg_text.jsonl](https://drive.google.com/file/d/1Bk90f0Rd982Px65GDuQayd5s8uXQs9UX/view?usp=sharing)       |       Text of provenance passages in the train topics.             |
| 	[2023_test_topics_psg_text.jsonl](https://drive.google.com/file/d/1YGhJAUEw9PPITPkrWP3EtnUsWiboh3eq/view?usp=sharing)       |       Text of provenance passages in the test topics.             |
| 	[2023_passages_hashes.tsv.bz2](https://ikattrecweb.grill.science/ikat_2023_passages_hashes.tsv.bz2)       |  TSV file containing MD5 hashes of passage texts. The `.tsv` file has this format: `doc_id   passage_number    passage_MD5`. Total download size is 2.2GB.           |
| 	[2023_top_1000_query_results.zip](https://drive.google.com/file/d/1csWaKo0WxZVACrMazlJLcW2Xy1msp9ec/view?usp=sharing)       |       This zip file has queries from both training and testing topics, saved in `queries_train.txt` and `queries_test.txt` respectively. The results from the [iKAT searcher](https://ikat-searcher.grill.science/) (`BM25` using manually resolved queries) are saved in the `query_results_train` and `query_results_test` folders. Each result file, with up to 1000 results, corresponds to a query based on line numbers, starting from zero. For instance, the results for the first query in `queries_train.txt` can be found in `query_results_train/query_results_000.txt`. In each result file, every line shows the `ClueWeb22 ID` followed by the `URL`.            |
| 	[ret_bm25_rm3--type_automatic--num_ptkb_3--k_100--num_psg_3.official.run.json](https://drive.google.com/file/d/14MyYzEYI7D5rBIsRjYfokyMU-9MQwm6t/view?usp=sharing)       |       Automatic  Run   |
| 	[ret_bm25_rm3--type_manual--num_ptkb_3--k_100--num_psg_3.official.run.json](https://drive.google.com/file/d/1fCjGmULmYoWimr5cgbN_suQ3hAN8YVNs/view?usp=sharing)       |      Manual  Run           |


## **Baseline Runs iKAT 2023**

We provide two baseline runs (linked above). 

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


## **Data from TREC CAsT**
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


