## Portfolio

---

### Bi-LSTM-CRF Keyphrase Extractor

#### Summary

As an Insight Fellow, I built the database and model for a healthcare communication company to store and extract keyphrases from the abstracts of scientific literature on PubMed. The company needs a database of certain PubMed records annotated with keyphrases. However, less than 26% of the records come with author provided keyphrases.  To make use of the existing author annotated keyphrases and the future company annotated keyphrases, I built a supervised learning model that was trained on these existing records to extract keyphrases from the literature abstracts. Finally, to help the company better filter and index these records with their keyphrases, I built Elasticsearch database with Kibana and clio-lite powered frontend for easy web query and visualization.

#### Some technical details

<img src="images/dummy_thumbnail.jpg?raw=true"/>
I first started with tradtional unsupervised keyphrase extraction models like TF-IDF and TextRank, however, their performances are not good enough for the company. I then built the keyphrase extraction model as a sequence tagging model and used Bi-LSTM neural net architecture, which achieved much better performance. By adding a Conditional Random Field model that takes the LSTM output to calculate its emission score and builds relationship among adjacent predicted labels, the model performance was furhter improved. Noticing that the many out of vocabulary keyphrases not tagged by the model, I switched to use BioWordVec which includes sub-word embeddings and was trained on the PubMed database to furhter improve model performance. 

The model was fully modularied and implemented into the MlFlow framework to allow for monitoring, results and model storage, and easy deployment. 

#### Results and more

Final models yields a % F1 score, which is not too far from the current state of the art result achieved on the test dataset. The company has moved some of the visualization work flow from Power BI to Kibana in preparating to have their own platform.



Presentation slides can be found [here](/pdf/sample_presentation.pdf).
Repo for the code is [here](/pdf/sample_presentation.pdf).

---

### HackerNews Topic Modelling and Auto-encoder based Recommendation Engine

#### Summary

As an Insight Fellow, I built the database and model for a healthcare communication company to store and extract keyphrases from the abstracts of scientific literature on PubMed. The company needs a database of certain PubMed records annotated with keyphrases. However, less than 26% of the records come with author provided keyphrases.  To make use of the existing author annotated keyphrases and the future company annotated keyphrases, I built a supervised learning model that was trained on these existing records to extract keyphrases from the literature abstracts. Finally, to help the company better filter and index these records with their keyphrases, I built Elasticsearch database with Kibana and clio-lite powered frontend for easy web query and visualization.

#### Some technical details


I've obtained around 2 Million HackerNews posts from Google's BigQuery platform. First model I tried for the topic modelling is the LDA model with processed texts. However, since each entry/document in the input (the combined texts of post title, content, and link domain) contain only few words, LDA, which relies co-occurent of words to determine topic distributions, do not work well on these short text classification tasks. Pre-trained language models like BERT can generate constexulized sentence embeddings which provides numerical representation of the sentence which emcompasses semantic realtionships between the represented sentence and the corpus (Wikipeida+BookCorpus) BERT trained on. We can use these representations for an unsupervised clustering model for topic modelling. 

To possibly improve topic modelling performance, I concated the LDA output(topic distributions for each post) with the Distilled-BERT embeddings, used auto-encoder for dimensionality reduction and finally clustered the results into different topics. I then run TF-IDF to get the key words of these topics. Another way to get key words is to find the words with embeddings that have large cosine similarity to the topic embeddings (averaged sentence embeddings), which is much more time consuming.

In the second part of the project, I try to find the unusual posts for recommendation using an auto-encoder model for each topic. The auto-encoder takes the posts embeddings as input, reduce them to smaller dimensions, and then reconstruct the posts embeddings from these smaller dimension representations. The unusual posts are defined as posts that are harder to reconstruct with the decoders of auto-encoders, which suggest that they differ from other posts in this topic. I have tested using both sentence level embedding (in a neural network with all dense layers) and word level embeddings (in a neural network utilizing LSTM or CNN structures) as the auto-encoder input. The results are much better using word level embeddings. Another observation is that auto-encoders could also filter out lower quality posts (like advertisements), to futher improve model performance, an auto-encode should be built on the whole dataset, and filter out these posts before doing topic modelling and recommendation. Also, LSTM and CNN auto-encoders perform similarly on this taks, although CNN ones paralleied much better are faster to run.

Repo for this project is hosted here

