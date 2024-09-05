
We need to differentiate between two kinds of models.
1) Bidirectional models (attention?) that are used to create embeddings for each token taking into account the context. (ada-2, many in hugging face)
2) Language model (only forward) that are used to generate the next token. (gpt, llama, etc)

For RAG we need  steps:  

First , create embeddings for our documents for retrieval. This means using some model of class 1 to generate embeddings for each token in each corresponding context. Then, (not sure how openai operates in the background) but in general, we generate an embedding for each document (or unit that we are using, could be a paragraph or whatever). That's done by summarizing the token level embedding to a document level embedding (pooling for ex-> average of the embeddings of the document) so you end up with just one embedding of the defined dimension. FV also mentioned tokenCLS that is some kind of token defining the document but not sure how that's created.  

Second, storing those embeddings. So each document will have an embedding (document / paragraph / whatever). 

Third. When the user asks a question we need to use the same 1) model to create the embedding for the question (token level to question level) and then look for the documents in our store that the most similar (KNN) to the question one. This will retrieve X most similar documents (in text format again)

Fourth. Now we have the similar documents that might answer our question. We use the model 2) to give back the question, any other background prompt (format, phrasing, etc) and the retrieved documents, also being part of the general prompt now (context)

Fifth. The model 2) will again convert all this prompt + context into embeddings (but the ones that are associated with this model) and will perform generation of the answer based on the embeddings. This is a forward model, since it needs to generate text. So, at this stage the model will answer the question as it was trained to do that and the embeddings it generated are associated with that.



So, each model, of any kind, will generate it's own embeddings, which are related to how it was trained and the task it's supposed to solve. For 1), a best practice is going into huggingface and look for models that perform best in the kind of task we are aiming (some are good for similarity in text associated with objects, other with novels (??) check hugging face for this). The idea is to pick the right model , **and this is critical**, so when we are calculating similarities for retrieval , the most similar will be the best to actually later answer the question.

----
Thoughts regarding youtube.
We should create many files and not just one, probably now it's retrieving everything and not just parts of the txt, because there is only one embedding.
Maybe create a document per paragraph or similar, and hope that the retrieval will get only a few paragraphs associated with the question.
We should see which models are available for embedding also.
We should check if it makes sense to have our own db with any model that we want.