# **Datasets and Resources**

## :boom: **Topics for iKAT Year 1**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2023_train_topics.json](https://drive.google.com/file/d/1sNHmVYO9PVG2kFxLscPGhN-uCCUuDAu9/view?usp=sharing)       |       Train topics in JSON format.             |
| 	[2023_train_topics_psg_text.jsonl](https://drive.google.com/file/d/1Bk90f0Rd982Px65GDuQayd5s8uXQs9UX/view?usp=sharing)       |       Text of provenance passages in the train topics.             |
| 	[2023_test_topics.json](https://drive.google.com/file/d/1S4e1MjWSkCsfXfp7fAdyqTNnFMs2WIhI/view?usp=sharing)       |       Test topics in JSON format.             |
| 	[2023_test_topics_psg_text.jsonl](https://drive.google.com/file/d/1YGhJAUEw9PPITPkrWP3EtnUsWiboh3eq/view?usp=sharing)       |       Text of provenance passages in the test topics.             |

## :boom: **Collection: TREC iKAT 2023 ClueWeb22-B**

The collection distribution is being handled directly by CMU and not the iKAT organizers. Please follow these steps to get your data license ASAP:

- Sign the license form available on the [ClueWeb22 project web page](https://lemurproject.org/clueweb22/obtain.php).
- Send the form to CMU for approval (**jlm4@andrew.cmu.edu**)

Please give enough time to the CMU licensing office to accept your request. A download link will be sent to you by the ClueWeb22 team at CMU.

**Note.** 

- CMU requires a signature from the organization (i.e., the university or company), not an individual who wants to use the data. This can slow down the process at your end too. So, it’s useful to start the process ASAP.
- If you already have an accepted license for ClueWeb22, you don’t need a new form. Please let us know if that’s the case.


## :boom: **Additional Resources**

We provide the following additional resources for the teams:

- **Document segments in `JSONL` format.**
	- These files contain passages in the form `{"id": "[passage id]", "contents": "[passage text]", "url": "[ClueWeb22 document URL]"}`
	- Passage IDs are structured as: `doc_id:passage_number`
	- Total download size is approximately 31 GB.
	- Total number of passages is 116,838,987
- **Document segments in `TrecWeb` format.**
	- Format as shown on website.
	- Total download size is approximately 31 GB
- **Passage hashes.**
	- TSV file containing MD5 hashes of passage texts.
	- The `.tsv` file has this format: `doc_id   passage_number    passage_MD5`
	- Total download size is 2.2 GB
- **Passage index.**
	- Lucene index generated from the `JSONL` passage files above using Pyserini.
	- The files form a single `.tar.bz2` archive split into sections for simpler downloading due to the overall size.
	- To extract the archive, once downloaded, you must combine each of the sections in name order back into a single file:
	`cat ikat_2023_passage_index.tar.bz2.part* > ikat_203_passage_index.tar.bz2`
	- Total download size is approximately 150 GB
	
**How do I access this data?** Each team should use a URL of `https://ikattrecweb.grill.science/<team_name>` to access the files. The page will ask for a userID and password. Enter the login details which you obtained from the iKAT organizers. You should see a page which lists each type of data and has links to the individual files listed above, along with their checksum files.

**NOTE:** 

- Currently, these teams can access this URL: `MLIA`,  `TREMA_UNH` and `Nota` . Please send us a message privately via slack or through the email and we will share the login details with you.
- Other teams: You have shared IPs in the `10.x.x.x` range which is for private networks, so we need another IP from you. Can you please share another suitable IP with us so that we may configure the above download link to work for you?



## **Additional Data from TREC CAsT**
Those who are eager to start developing and experimenting can also use the data from previous years' TREC CAsT. The iKAT topics are going to be similar, with the addition of the Personal Text Knowledge Base. For more information on TREC CAsT, [see the website](https://www.treccast.ai/) and read the overview papers [[2019]](https://arxiv.org/abs/2003.13624) [[2020]](https://scholar.google.com/scholar_url?url=https://www.cs.cmu.edu/afs/cs.cmu.edu/Web/People/callan/Papers/trec2021-dalton.pdf&hl=en&sa=T&oi=gsb-gga&ct=res&cd=0&d=4712262702816537608&ei=En6IZNaeLcPFmAGMo47oDQ&scisig=AGlGAw9ggvQnzye9tQlrvoob1Js5) [[2021]](https://www.cs.cmu.edu/~callan/Papers/trec22-Jeffrey_Dalton.pdf) [[2022]](https://trec.nist.gov/pubs/trec31/papers/Overview_cast.pdf)

**Note.** TREC CAsT did not include a PTKB but you can be creative and modify the data according to your needs. Also, TREC CAsT used different collections (Wikipedia, KILT, MS MARCO, etc.) at different stages. **_iKAT will be using a subset of the recently released ClueWeb22-B_**.

Below, we provide links to the TREC CAsT topics from previous years for participants. 

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

