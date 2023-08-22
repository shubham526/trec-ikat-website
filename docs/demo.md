# **TREC iKAT 2023: Getting Started**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1MOKGQZxjDhREX6d-m68om9mzTmHbwLsa?usp=sharing)

---


## **Objective**
To help you get started, we (the iKAT organizers) have put together this guide. In this demo, we'll explore and build the components of a simple iKAT system. These components include:

- Query Rewriter
- Passage Retriever, and
- Response Generator

## **System Architecture**

<center>
<img src="https://drive.google.com/uc?export=view&id=1iYVORhPemdFd6yR2S_0EP2L8l0K_nBP3
" width="85%"/>
</center>

The diagram above shows how the components of our system interact.

Given a query, conversation context, and the PTKB of the user, our system's **Query Rewriter** reformulates the query to resolve ambiguity. Next, the **Passage Retriever** uses the reformulated query to retrieve the top-K candidate passages from an index. Finally, the **Response Generator** uses the top-N of the K retrieved passages to generate a coherant response. The output of our system is a response along with the provenance relevant passages used to construct the response for the input query, based on the conversation context.


## **Setup**



Before putting our system together, let's download the topics and the demo collection.



### **TREC iKAT 2023 Simple English Wikipedia Passage Collection**


Downloading and processing the entire TREC iKAT 2023 ClueWeb22-B passage collection is not possible on Colab. Moreover, it requires a licenece to use. For this demo, we will use [Simple English Wikipedia](https://simple.wikipedia.org/wiki/Main_Page). Compared to the full English wikipedia, it has only about 170k articles. The iKAT organizers have preprocessed the articles and created a passage collection for you to use. This collection is in a `jsonl` format. An example record from the collection is shown below:
```
{
    "id": "simplewiki:Ted%20Cassidy:0",
    "contents": "Ted Cassidy (July 31, 1932 - January 16, 1979) was an American actor. He was best known for his roles as Lurch and Thing on \"The Addams Family\".",
    "title": "Ted Cassidy",
    "wiki_id": "9822"
}
```

Each record in this collection contains the following fields:

1. `id`: The passage id is a combination of (1) the string "simplewiki:", (2) the encoded title of the Wikipedia page, and (3) the passage number. This is similar to the iKAT 2023 passage id format (***doc_id:passage_number***) 
2. `contents`: The text of the passage.
3. `title`: The title of the Wikipedia page to which this passage belongs.
4. `wiki_id`: The Wikipedia page ID of the Wikipedia page to which this passage belongs. These IDs are unique and will never change


> **Note.** As this collection is a toy collection meant for demo purposes, the quality of results we obtain in this tutorial may be affected.


```python
!pip install gdown
```


```python
!echo "Creating target directory.."
!mkdir -p ikat_demo
!mkdir -p ikat_demo/collection

import gdown
# The Google Drive file ID and the destination path
url = 'https://drive.google.com/uc?id=1touBjwkPByH69utT9_sevr5nYT0TTZ2M'
output = '/content/ikat_demo/collection/simplewiki-2020-11-01.passages.jsonl'
gdown.download(url, output, quiet=False)

url = 'https://drive.google.com/uc?id=1zPSiAqLmbx9QFGm6walnuMUl7xoJmRB7'
output = '/content/ikat_demo/test.json'
gdown.download(url, output, quiet=False)
```

## **Creating a BM25 Index**



Now, we'll use the [Pyserini information retrieval toolkit](https://github.com/castorini/pyserini) to build a sparse index for the collection we just downloaded. Pyserini provides APIs for our indexing needs and supports both sparse and dense retrieval. Alternatively, you may also use [PyTerrier](https://github.com/terrier-org/pyterrier).

First, let's install Pyserini and its dependcies.


```python
!pip install pyserini
!pip install faiss-cpu
```

Pyserini provides ingestors for document collections in many different formats. The simplest, however, is the following JSON format:
```
{
  "id": "doc1",
  "contents": "this is the contents."
}
```
The collection to be used with Pyserini must be in a `jsonl` format, where each line is a `json` record structured as above.
The preprocessed collection that we provide is already in a `jsonl` format.


```python
!python -m pyserini.index.lucene \
  --collection JsonCollection \
  --input  '/content/ikat_demo/collection/' \
  --index '/content/ikat_demo/index' \
  --generator DefaultLuceneDocumentGenerator \
  --threads 8 \
  --storePositions --storeDocvectors --storeRaw
```

To check that our new sparse index works, let's try searching with it. The code below loads the index and searches for the query `global warming`.


```python
from pyserini.search.lucene import LuceneSearcher

searcher = LuceneSearcher('ikat_demo/index')
query = 'global warming'
hits = searcher.search(query)

for i in range(len(hits)):
    print(f'{i+1:2} {hits[i].docid:4} {hits[i].score:.5f}')
```

Let's see the contents of the best ranking document.


```python
import json
best_ranked_doc = searcher.doc(hits[0].docid)
parsed_doc = json.loads(best_ranked_doc.raw())
parsed_doc['contents']
```

## **Query Rewriting**



iKAT topics mimic real-world dialogue phenomena. As a result, utterances within topics become increasingly ambiguous as the topic unfolds. On their own, these utterances likely won't return good candidates from our index, so we need to reformulate them using information from prior turns in the topic.

Let's examine the utterances in a topic to demonstrate the need for query rewriting. As an example, we will examine a segment from the iKAT 2023 test topic on "Finding a diet" conversation.

```
{
    "turn_id": 1,
    "utterance": "Hey, Assistant! I am wondering what to cook for dinner tonight for my family.",
    "resolved_utterance": "Hey, Assistant! I am wondering what to cook for dinner tonight for my family.",
    "response": "Hey there! There are many healthy and delicious recipes you could try! For example, a stir-fry with lean proteins like chicken or tofu and a variety of colorful vegetables is always a good option. Or you could try making a Mediterranean quinoa salad with cucumbers, tomatoes, feta cheese, and a light lemon vinaigrette. Baked salmon with a side of roasted vegetables is also a great choice.",
},
{
    "turn_id": 2,
    "utterance": "Nah, we had salmon just two days ago. But the Mediterranean dish that you mentioned sounds interesting, could you provide the recipe for that?",
    "resolved_utterance": "Nah, we had salmon just two days ago. But the Mediterranean quinoa salad sounds interesting, can you provide the recipe for the Mediterranean quinoa salad?",
    "response": "Ah, I see. Of course! Here's a simple recipe for Mediterranean Quinoa Salad. The ingredients you need are 1 cup quinoa, 2 cups water, 1/4 cup extra-virgin olive oil, 2 limes, juiced, 2 teaspoons ground cumin, 1 teaspoon salt, 1/2 teaspoon red pepper flakes (optional), 1 1/2 cups halved cherry tomatoes, 1 cucumber, diced, 1/2 cup chopped fresh mint, 1/2 cup feta cheese. Instructions: Bring quinoa and water to a boil in a saucepan. Reduce heat to medium-low, cover, and simmer until quinoa is tender and water has been absorbed, 10 to 15 minutes. Set aside to cool. In a large bowl, whisk olive oil, lime juice, cumin, salt, and red pepper flakes together to create the dressing. In the bowl with the dressing, mix in quinoa, tomatoes, cucumber, and mint. Stir until well mixed. Top with feta cheese before serving. Enjoy!"
},
{
    "turn_id": 3,
    "utterance": "That's great, thanks! But I'm thinking of reducing the amount of oil in the dressing. Any suggestions?",
    "resolved_utterance": "That's great, thanks! But I'm thinking of reducing the amount of oil in the salad dressing. Any suggestions?",
    "response": "Certainly! You can reduce the amount of oil by using more lime juice or adding some water. Alternatively, you could substitute some of the oil with a different kind of liquid like vinegar. This should maintain a good balance between tartness and the need for oil as an emulsifier. Which of these do you think works the best for you?"
},
{
    "turn_id": 4,
    "utterance": "Thanks! My children don't like vinegar, so I would probably go with lime juice. Can I also use butter instead?",
    "resolved_utterance": "Thanks! My children don't like vinegar, so I would probably go with lime juice. Can I also butter instead of oil in the Mediterranean Quinoa Salad?",
    "response": "Butter can be substituted for vegetable oil in some dishes, but not always. Butter is a solid fat, while vegetable oil is liquid, so they have different effects on the consistency and texture of the food. Butter is also higher in calories and lower in trans fats than vegetable oil. If you want to use butter instead of vegetable oil, you may need to adjust the quantity, melt it down, and let it cool a bit. For example, 1 cup of butter equals 7/8 cup of vegetable oil."
},
{
    "turn_id": 5,
    "utterance": "I see, thanks! What are some cheese substitutes?",
    "resolved_utterance": "I see, thanks! What are some substitutes for cheese in the Mediterranean Quinoa Salad?",
    "response": "I can suggest some alternatives for feta cheese. One option is to use a lower fat cheese like mozzarella. Another is to use a plant-based cheese substitute, if you're open to vegan options. You could also try adding a bit of nutritional yeast for a cheese-like flavor with less fat and more protein."
},
{
    "turn_id": 6,
    "utterance": "Can you compare the first two?",
    "resolved_utterance": "Can you compare mozzarella with plant-based cheese?",
    "response": "Mozzarella cheese is a dairy product derived from milk and produced in a wide range of flavors. It comprises fat, usually from the milk of cows, buffalo, goats, or sheep. On the other hand, vegan cheese is a non-dairy, plant-based product. Vegan cheese is 100 percent animal-free cheese and is made using vegetable proteins. The ingredients of vegan cheese include soy, nuts, cashews, and vegetable oils (such as coconut oil). Vegan cheese is made using a process similar to traditional cheese making-without the animal milk. During the process, the plant proteins are separated using bacteria. Then the ingredients, such as oils and thickeners, are added to achieve the desired cheese-like consistency."
},
{
    "turn_id": 7,
    "utterance": "Which of these is suitable for me?",
    "resolved_utterance": "Which of mozzarella and vegan cheese is suitable for me?",
    "response": "Based on the nutritional facts, mozzarella can be considered safe for lactose-intolerant people to eat, but be aware that the type of mozzarella you buy may not help reduce your symptoms. This is because many types of processed and packaged mozzarella also contain whey powder or casein - two other forms of dairy which are high in lactose. However, mozzarella has almost no lactose. Just one ounce of cheese provides around 0.3 grams of lactose. Vegan cheese is also a good alternative for lactose-intolerant people. Vegan cheeses are 100 percent animal-free and made using vegetable proteins. There are many different types of vegan cheese available, including vegan mozzarella. So, both mozzarella and vegan cheese can be suitable for lactose-intolerant people. It depends on your personal preference and dietary needs."
},
```


This topic starts with a question regarding selecting a diet. If we isolate `Turn 6` from the rest of the conversation and use it for search, we would likely get minimal, if any, results.

Now, let's see how a query rewriter helps.

We'll use a [`T5` query rewriter from `HuggingFace`](https://huggingface.co/castorini/t5-base-canard). It is finetuned on the [`CANARD` dataset](https://sites.google.com/view/qanta/projects/canard) but works effectively on iKAT queries.


```python
# Load model and tokenizer from HuggingFace
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM
import torch

device = "cuda" if torch.cuda.is_available() else "cpu"
rewriter = AutoModelForSeq2SeqLM.from_pretrained("castorini/t5-base-canard").to(device).eval()
rewriter_tokenizer = AutoTokenizer.from_pretrained("castorini/t5-base-canard")
```

 The model rewrites an utterance using that utterance and all previous utterances and system responses as input. The utterance and previous turn utterances and system responses should be separated by `|||` when building the input to the model.


Let's read the `json` data file and load the turns.


```python
with open('/content/ikat_demo/test.json', 'r') as f:
    topics = json.load(f)
```

Next, we write a small function to extract the context.

The provided Python function, `extract_context`, extracts a sequence of utterances and responses up to a given `turn_id` from a JSON data structure. Here's a breakdown:

1. **Purpose:** Extracts a series of utterances and responses up to a specified turn from a given JSON data based on the provided `number`.

2. **Parameters:**
	- `json_data`: A list of dictionaries, where each dictionary represents a conversation that has a unique number and contains a series of turns.
	- `number`: The unique identifier for a specific conversation in the JSON data.
	- `turn_id`: A specified turn up to which the utterances and responses will be extracted.

3. **Process:**

	a. **Locate Conversation:** Loops through the `json_data` to find the dictionary with the given `number`.

   	b. **Error Handling:** If no dictionary with the given `number` is found, it returns a message indicating so.

   	c. **Extracting Text:** Loops through the turns within the found conversation and appends the utterances and responses up to the `turn_id` to a list.

   	d. **Context Formation:** Concatenates the extracted utterances and responses using "|||" as a separator to form the context.

4. **Output:** A tuple containing:

	- The current `utterance` for the provided `turn_id`.
	- The `context`, which is the sequence of utterances and responses up to the given `turn_id`, concatenated with "|||".


```python
def extract_context(json_data, number, turn_id):
    # Find the correct dictionary with the given number
    data = None
    for item in json_data:
        if item['number'] == number:
            data = item
            break

    # If we couldn't find the data for the given number
    if not data:
      print("No data found for the given number.")
      return "No data found for the given number.", None

    # Extract the utterance and response values
    texts = []
    current_utterance = ""
    for turn in data['turns']:
        if turn['turn_id'] < turn_id:
            texts.append(turn['utterance'])
            texts.append(turn['response'])
        elif turn['turn_id'] == turn_id:
            current_utterance = turn['utterance']
            texts.append(current_utterance)

    # Join the texts with "|||" separator
    context = '|||'.join(texts)

    return current_utterance, context
```

Now we can use this function to extract the context for a given topic `number` and `turn_id` in the topic.


```python
number_to_search = "10-1"
turn_id_to_search = 6
utterance, context = extract_context(topics, number_to_search, turn_id_to_search)
print(f"Raw Utterance: {utterance}")
print(f"Turn Context: {context}")
```



> **NOTE:** When building context this way, there's a risk that the input can become too lengthy for subsequent interactions, especially in extended discussions. For handling this, you can experiment with various context truncation methods. A straightforward strategy is to eliminate earlier turn utterances and responses if the input size surpasses the model's token limit.

Now, let's rewrite the query using our model.


```python
def rewrite_query(context: str, model, tokenizer, device) -> str:
  tokenized_context = tokenizer.encode(context, return_tensors="pt").to(device)
  output_ids = model.generate(
      tokenized_context,
      max_length=200,
      num_beams=4,
      repetition_penalty=2.5,
      length_penalty=1.0,
      early_stopping=True
  ).to(device)

  rewrite = tokenizer.decode(output_ids[0], skip_special_tokens=True)
  return rewrite
```


```python
rewrite = rewrite_query(context, rewriter, rewriter_tokenizer, device)
print(f"Raw Utterance: {utterance}")
print(f"Query Rewrite: {rewrite}")
```

Hmm, that didn't really help! 😞 The rewriter did expand the query but with the wrong information!

## **Expanding the Context using Relevant PTKB Statements**



One major difference between iKAT and CAsT is the presence of the Personal Text Knowledge Base (PTKB). In the first year, we are providing the PTKB as a dictionary of statements about the user. Each PTKB defines a user's profile and controls how the system should respond to the user. For the example conversation above, the PTKB, as provided in the test data, is as below.
```
{
    "1": "I want to know about healthy cooking techniques.",
    "2": "I am lactose intolerant.",
    "3": "I'm looking for a speaker set to match my TV.",
    "4": "I'm willing to drive a long distance to find a cheaper TV.",
    "5": "I'm hoping to find some offers and discounts for TV.",
    "6": "I like to eat fruits and vegetables.",
    "7": "I don't read much.",
    "8": "I want to cook healthy and tasty recipes for my family.",
    "9": "I am on a diet and prefer low-calorie food.",
    "10": "I want to know about the nutritional value of the ingredients I use.",
    "11": "I'm looking for a new TV to replace my current one.",
    "12": "I want a TV that is okay for light and size of my living room."
},
```
Above, we re-wrote the query using the context. But for a more persoanlized conversation, one approach to query rewriting could be to use the PTKB statements in the query reformulation process.

To incorporate the PTKB into the system, we must answer two questions:

1. What are the relevant PTKB statements for the current turn?
2. How do we use these relevant PTKB statements?



### **Question 1: What are the relevant PTKB statements for the current turn?**



In a `manual` run, you may use the `ptkb_provenance` fields. This field was manually populated by the iKAT topic developers and provides a straightforward way to identify relevant PTKB statements for the given turn utterance. However, a more difficult (and perhaps interesting) exercise is to automatically identify relevant PTKB statements for the given turn.

One easy-to-implement (and probably good) solution is to use `BERT` embeddings. Specifially, we can use `SentenceTransformers`

[`SentenceTransformers`](https://www.sbert.net/index.html) is a Python framework designed for sentence, text, and image embeddings. the foundational work on this was presented in the paper titled [Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks](https://arxiv.org/abs/1908.10084).

This tool enables computation of sentence and text embeddings in over 100 languages. You can then use cosine similarity, for instance, to identify sentences of similar meanings. It's particularly valuable for semantic text similarity, semantic searching, and paraphrase detection.

Built on [PyTorch](https://pytorch.org/) and [Transformers](https://huggingface.co/docs/transformers/index), the framework boasts a vast array of pre-trained models optimized for diverse tasks. Moreover, fine-tuning your models is a breeze.

We are going to use the `CrossEncoder` model from `SentenceTransformers` to identify the relevant PTKB statements. Specifically, we are going to **re-rank** the PTKB statements based on the current utterance.

A `CrossEncoder`-based re-ranker can significantly enhance the end results for users. In this approach, both the query and a potential document are fed into the transformer network concurrently. The network then produces a score between 0 and 1, signifying the document's relevance to the query.

The strength of a `CrossEncoder` lies in its superior performance, stemming from its ability to execute attention operations across both the query and the document.

We will use [`cross-encoder/ms-marco-MiniLM-L-6-v2`](https://huggingface.co/cross-encoder/ms-marco-MiniLM-L-6-v2) model from HuggingFace that scores the query and all retrieved passages for their relevancy.

For a complete introduction to using cross encoders and retrieval and reranking, [see this notebook](https://github.com/UKPLab/sentence-transformers/blob/master/examples/applications/retrieve_rerank/retrieve_rerank_simple_wikipedia.ipynb).

First, we need to install the `SentenceTransformers` library


```python
!pip install sentence-transformers
```

Next, we write a small function that will rerank the PTKB statements for the given query.

The provided Python function, `get_ptkb_statements`, compares statements from the PTKB with a query to determine their similarity. Here's a step-by-step explanation of the function:

1. **Purpose:** The function aims to return the top `num_ptkb` statements from the PTKB that are most similar to the given `query`.

2. **Parameters:**
   	- `query`: The user's input or question.
   	- `num_ptkb`: The number of PTKB statements to return.
   	- `ptkb`: A dictionary of the PTKB statements.
   	- `reranker`: A model that predicts the similarity score between two texts.

3. **Process:**
   
   	a. **Calculate Similarity Scores:** For each statement in the PTKB, it computes a similarity score with the `query` using the `reranker`. The score is between 0 and 1, with 1 being highly similar.

   	b. **Pair Statements with Scores:** The statements from the PTKB are paired with their respective similarity scores.

   	c. **Sort Pairs:** The pairs are then sorted in descending order based on their similarity scores.

   	d. **Extract Statements:** From the sorted pairs, the actual statements are extracted.

   	e. **Return Top Statements:** The top `num_ptkb` statements are then concatenated into a single string and returned.

4. **Output:** A string containing the top `num_ptkb` statements from the PTKB that are most similar to the given `query`, separated by spaces.


```python
def get_ptkb_statements(query, num_ptkb, ptkb, reranker):
    # Find the similarity of PTKB statements with the given query
    similarity_scores = [reranker.predict([[query, ptkb_statement]])[0] for ptkb_statement in ptkb.values()]

    # Pair each statement with its similarity score
    statement_score_pairs = list(zip(list(ptkb.values()), similarity_scores))

    # Sort the pairs based on the similarity scores in descending order
    sorted_pairs = sorted(statement_score_pairs, key=lambda x: x[1], reverse=True)

    # Extract the sorted responses
    sorted_ptkb_statements = [pair[0] for pair in sorted_pairs]

    # Return required number of PTKB statements
    return ' '.join(sorted_ptkb_statements[:num_ptkb])
```

Now, let's use this function to find the top relevant PTKB statements for a given turn.


```python
query = "Can you compare the first two?"
ptkb = {
    "1": "I want to know about healthy cooking techniques.",
    "2": "I am lactose intolerant.",
    "3": "I'm looking for a speaker set to match my TV.",
    "4": "I'm willing to drive a long distance to find a cheaper TV.",
    "5": "I'm hoping to find some offers and discounts for TV.",
    "6": "I like to eat fruits and vegetables.",
    "7": "I don't read much.",
    "8": "I want to cook healthy and tasty recipes for my family.",
    "9": "I am on a diet and prefer low-calorie food.",
    "10": "I want to know about the nutritional value of the ingredients I use.",
    "11": "I'm looking for a new TV to replace my current one.",
    "12": "I want a TV that is okay for light and size of my living room."
}
num_ptkb = 3
```


```python
from sentence_transformers import CrossEncoder
reranker = CrossEncoder('cross-encoder/ms-marco-MiniLM-L-6-v2')
ptkb_statements = get_ptkb_statements(query, num_ptkb, ptkb, reranker)
ptkb_statements
```

### **Question 2: How do we use these relevant PTKB statements?**

One possible way of using these relevant PTKB statements is to include them in the context when re-writing the query.

Let's see how that works. We will modify out previous function `extract_context` a little to include the relevant PTKB statements.


```python
def extract_context_with_ptkb_statements(json_data, number, turn_id, ptkb_statements):
    # Find the correct dictionary with the given number
    data = None
    for item in json_data:
        if item['number'] == number:
            data = item
            break

    # If we couldn't find the data for the given number
    if not data:
        print("No data found for the given number.")
        return "No data found for the given number."

    # Extract the utterance and response values
    texts = [ptkb_statements]
    current_utterance = ""
    for turn in data['turns']:
        if turn['turn_id'] < turn_id:
            texts.append(turn['utterance'])
            texts.append(turn['response'])
        elif turn['turn_id'] == turn_id:
            current_utterance = turn['utterance']
            texts.append(current_utterance)

    # Join the texts with "|||" separator
    context = '|||'.join(texts)

    return current_utterance, context
```


```python
number_to_search = "10-1"
turn_id_to_search = 6
utterance, context = extract_context_with_ptkb_statements(topics, number_to_search, turn_id_to_search, ptkb_statements)
print(f"Raw Utterance: {utterance}")
print(f"Turn Context: {context}")
```


```python
rewrite = rewrite_query(context, rewriter, rewriter_tokenizer, device)
print(f"Query Rewrite: {rewrite}")
```

That didn't help either! 😞

This is a really difficult query for the system! We are excited 🤩 to see how **your** system handles such queries.

Alternatively, we can also append the PTKB statements to the rewritten query (without PTKB statements).

## **Passage Retrieval and Reranking**

In iKAT 2023, we provide several tasks, [see the guidelines](https://www.trecikat.com/guidelines/) section of the webpage for more details.

One core task in iKAT 2023 involves producing a ranked list of relevant passages corresponding to a specific user utterance. During the ***Passage Retrieval*** phase, we employ the rephrased query (either manually or automatically adjusted) to fetch a potential set of passages from the previously generated sparse index.

The *retrieve-then-rerank* approach is a widely adopted strategy in Information Retrieval systems, aimed at enhancing the quality of the preliminary set of candidates. The process commences with a swift and effective retrieval method to fetch the initial set of passages. One prevalent method for this is BM25. However, there's also the option of adopting dense retrieval methods like Bi-encoders. For a comprehensive understanding of utilizing bi-encoders in retrieval, consider checking [this guide](https://www.sbert.net/examples/applications/retrieve_rerank/README.html).

Subsequent to this initial retrieval, the candidate set undergoes a reranking process, leveraging more advanced methods. An example would be rerankers rooted in BERT, known as cross-encoders. In this tutorial, we'll specifically employ the `CrossEncoder` from the `SentenceTransformers` library.



### **Step-1: Retrieval using BM25**

We will first retrieve a candidate set of passages from our index using BM25. As query, we will use the manually resolved utterance from `turn_id=6` in the example shown above.


```python
def retrieve_using_bm25(query):
    hits = searcher.search(query)
    candidate_set = []
    for i in range(len(hits)):
        print('Rank: {} | PassageID: {} | Score: {}'.format(i+1, hits[i].docid, hits[i].score))
        doc = searcher.doc(hits[i].docid)
        parsed_doc = json.loads(doc.raw())
        print(parsed_doc['contents'])
        candidate_set.append({
            'passage_id': hits[i].docid,
            'bm25_rank': i+1,
            'bm25_score': hits[i].score,
            'passage_text': parsed_doc['contents']
        })
        print('=================================')
    return candidate_set
```

### **Step-2: Rerank using CrossEncoder**

Next, we will rerank this candidate set using the `CrossEncoder` defined earlier.


```python
def rerank_passages(query, passages, reranker):
    res = []
    query_passage_pairs = [[query, passage['passage_text']] for passage in passages]
    scores = reranker.predict(query_passage_pairs)

    for passage, score in zip(passages, scores):
        passage['reranker_score'] = score
        res.append(passage)

    ranked_passages = sorted(passages, key=lambda x: x['reranker_score'], reverse=True)
    return ranked_passages
```


```python
query = "Can you compare mozzarella with plant-based cheese?"
candidate_set = retrieve_using_bm25(query)
```


```python
import numpy as np
reranked_passages = rerank_passages(query, candidate_set, reranker)
print(json.dumps(reranked_passages, indent=4, default=lambda o: float(o) if isinstance(o, np.float32) else o))
```

These results are not great. An important thing to note here is that we are doing retrieval over a very small corpus of `SimpleEnglishWikipedia`. As mentioned earlier, the results may not be of high quality.

## **Response Generation**

One of the tasks in iKAT 2023 is response generation. After retrieval, the system should use the top-K passages to generate a short response (250 words or less) that is appropriate for an interactive conversational agent to give to the user.

Let's explore one way this can be done, by framing the task as a summarisation problem. We will use the `T5` model for this purpose. Specifically, we will use the `mrm8488/t5-base-finetuned-summarize-news` model from HuggingFace.

The [`mrm8488/t5-base-finetuned-summarize-news`](https://huggingface.co/mrm8488/t5-base-finetuned-summarize-news) is Google's `T5-base` model fine-tuned on the [News Summary dataset](https://www.kaggle.com/datasets/sunnysai12345/news-summary) for the downstream task of summarization.

First, we will write a short function for this task.

The `generate_response` function is described below:

1. **Purpose:** Generates a summarized response based on the top passages from a set of documents returned by a search operation.

2. **Parameters:**

	- `passages`: A set of top documents or hits returned by the search operation.
	- `model`: An instance of a pre-trained sequence-to-sequence language model (from the `AutoModelForSeq2SeqLM` class) for generating summaries.
	- `tokenizer`: An instance of a tokenizer (from the `AutoTokenizer` class) used to tokenize and decode text.

3. **Process:**

	a. **Consolidating Passages**: Combines all the extracted passages into one continuous string.

	b. **Tokenization and Input Formation**: Tokenizes the combined text and pre-processes it by adding a "summarize: " prefix. The tokenized input is adjusted to not exceed a specified maximum length (512 tokens) and is moved to the desired computation device.

	c. **Generating Summary**: Utilizes the sequence-to-sequence language model to generate a summarized response based on the input. Applies various parameters to control and improve the quality of the output summary.

	d. **Decoding the Summary**: Transforms the token IDs from the generated summary back into human-readable text, ensuring any special tokens are omitted.

4. **Output:** Returns a coherent and summarized text derived from the top passages of the documents.


```python
def generate_response(passages, model, tokenizer):
    text = ' '.join(passages)
    inputs = tokenizer.encode("summarize: " + text, return_tensors="pt", max_length=512, truncation=True)
    with torch.no_grad():
        summary_ids = model.generate(
            inputs,
            max_length=250,
            min_length=50,
            length_penalty=2.0,
            num_beams=4,
            early_stopping=True
        )
    summary = tokenizer.decode(summary_ids[0], skip_special_tokens=True)
    return summary
```


```python
summarizer = AutoModelForSeq2SeqLM.from_pretrained('mrm8488/t5-base-finetuned-summarize-news')
summarizer_tokenizer = AutoTokenizer.from_pretrained('mrm8488/t5-base-finetuned-summarize-news')
```


```python
# We use the top-3 reranked passages to generate a response
passages = [passage['passage_text'] for passage in reranked_passages][:3]
print(json.dumps(passages, indent=4))
```


```python
generate_response(passages, summarizer, summarizer_tokenizer)
```
