# **Datasets and Resources**

## :boom: **Topics for iKAT Year 1**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2023_train_topics.json](https://drive.google.com/file/d/1jXyiUkxAMWCVSXBv9qed5dgdx5vJe0r1/view?usp=sharing)       |       Train topics in JSON format. Contains 11 conversations spanning 8 topics.             |



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

