# **Datasets and Resources**
---

## :boom: **Topics for iKAT Year 2 (2024)**

|    File      |      Description      |
|:------------|:---------------------|
| 	[2024_test_topics.json](https://drive.google.com/file/d/1pIu_kSn6YHQRX74DFknmjUJA34-JY67M/view?usp=sharing)       |       Test topics in JSON format.             |

## **Additional Data**

We also provide [additional data](additional_data.md) from iKAT Year 1 and TREC CAsT 2019-2022. 


## **TREC iKAT ClueWeb22-B Passage Collection**

We provide a segmented version of the TREC iKAT ClueWeb22-B Document collection available from CMU in two formats: `JSONL` and `TrecWeb`. 

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
	
	
## **Pre-built Indices**

We provide the following indices to help participants get started.

| File | Description | Size (Approximate) |
|:---------------------|:---------------------------------------------------------------------------------------------------------------------------|:-------------------|
| Lucene passage index (`ikat_2023_passage_index.tar.bz2`)| Sparse Lucene index generated from the `JSONL` passage files above using Pyserini. The files form a single `.tar.bz2` archive split into sections for simpler downloading due to the overall size. To extract the archive, once downloaded, combine each of the sections in name order back into a single file using the following command: `cat ikat_2023_passage_index.tar.bz2.part* > ikat_2023_passage_index.tar.bz2`. | 150 GB |
| DeepCT Index (`pt_deepct.tar.bz2`) | [DeepCT](https://arxiv.org/abs/1910.10687) index built using [PyTerrier](https://github.com/terrierteam/pyterrier_deepct) | 70 GB |
| SPLADE Index (`pt_splade.tar.bz2`) | [SPLADE++](https://arxiv.org/abs/2205.04733) index built using [PyTerrier](https://github.com/cmacdonald/pyt_splade) | 138 GB | 
| SPLADE index (`splade_index.tar.bz2`) | [SPLADE++](https://arxiv.org/abs/2205.04733) index built using [official SPALDE code](https://github.com/naver/splade) | 97 GB | 

### **Code to build and search indices**

We open-source the code used to build the indices above. You can find the code [here](https://github.com/shubham526/ikat-2024/tree/main)

## **How do I access these resources?** 

Each team should use a URL of `https://ikattrecweb.grill.science/<team_name>` to access the files. The page will ask for a userID and password. Enter the login details which you obtained from the iKAT organizers. You should see a page which lists each type of data and has links to the individual files listed above, along with their checksum files.

**NOTE:** Please do not share IPs in the `10.x.x.x` range which is for private networks. We would need a suitable public IP so that we may configure the above download link to work for you.

## **iKAT Searcher**

iKAT Searcher is a simple tool developed to help with creating the topics for iKAT. The tool allows topic developers to visually assess the behaviour of a retrieval system, ultimately making it easier to develop challenging, but interesting, topics for the Track. You can interact with the system [here](https://ikat-searcher.grill.science/). See the [GitHub repository](https://github.com/andrewramsay/Interactive-CAsT/tree/deployment_testing).

## **Run Validation**

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
