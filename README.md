# CS_ML_hackathon

As a customer-obsessed hacker, your task is to tell whether an arbitrary question from customer already has an answer within a given documents pool of 10000+ documents.

### objectvie
The goal of this hackathon challenge is to predict if a given customer question can be answered by any of the given 10,322 document pool.
Note: Do not need to identify the documentm just have to predict a boolean 1 or 0 (answerability)

Example 

Input(query): How do I get internet on Amazon Fire?
Output(prediction): 1
Explanation: One of 10,332 documents contains “Connect Your Fire Tablet to Wi-Fi\n\nAccess Wi-Fi by following these steps.\n\n1.<Open the Settings menu.>\n2.<Set Airplane Mode to Off.>\n3.<Set the toggle next to Wi-Fi Mode to On.>\n4.<Select the network you want to connect to.>\n5.<A lock icon means that you need a Wi-Fi password: enter your Wi-Fi password then select Connect>” which could answer the given question

### Evaluation
We use F-score to evaluate the submission file with our ground truth values. Each group can do any number of submissions before the deadline and can view their score in ranked order in the public leaderboard. Since the groups can do multiple submissions and in order to avoid overfitting we only evaluate the submitted results on 25% of the test data and ranking is displayed on the public leader board. Once the deadline is closed the evaluation is done on the complete test set and final results are updated on the private leader board to announce the final winners.


### Methodology


1. QA Pipeline 

* step 1: relevant answer retrieval (top n doc retrieved)
    * query to document: BM 25, answerini
    * query to query: use semantics search and paraphrase mining techniques to get cosine-similarity score 
* step 2: query-document answerability classification
    * a BERT-small model fine tuned on 80% of training dataset. 
    * The model is trained as a binary classifier. Our classifier takes the first 512 tokens of the paragraph and the whole question as input and outputs whether the document can answer the question or not.

2. Language Model

* Inspired by the Closed Book Open Domain QA model (https://arxiv.org/pdf/2002.08910.pdf) that uses a T5 model pretrained on QA datasets to directly predict the answer to a question *without any external knowledge*
* For this problem, we proposed to use the same approach as the paper by pre-training a T5 model on the 10,332 document pool with the typical pre-training objective (though we also considered *salient span masking*, which provided significant improvement in the paper)
* Then, we would fine-tune the pre-trained model on the hackathon dataset to directly predict answerability. The key issue is that because there were no negative instances (i.e. all questions in our dataset were answerable), we would have to rely on external data to train the model to recognize negatives
* Another idea we proposed using language models were to train the model to predict a context snippet based on the question



References

Closed Book Language Model for QA: https://arxiv.org/pdf/2002.08910.pdf
Open Domain QA  https://arxiv.org/pdf/2009.00914.pdf
Bert passage ranking https://arxiv.org/pdf/1901.04085.pdf
Sentence-Bert: Sentence Embeddings https://www.aclweb.org/anthology/D19-1410.pdf
