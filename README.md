[Full readme is in the original repo]

Data Processing
--------------
Supposed having t-rex dataset preprocessed with corenlp for csv file generation.

To run the model the first thing to do is create a dataset.
You need a file like data-sample.csv.
The file must be tab-separated an with the following fields:

1. bag of words between entities in a sentence
2. first entity
3. second entity
4. trigger words
5. pos tags of tokens between e1 and e2
6. entity type of e1_e2
7. entity type of e1
8. entity type of e2
9. words on the syntactic dependency path between e1 and e2
10. relation/predicate link from wikidata

In order to create the dataset you need the OiePreprocessor.py script once for each dataset partition: train, dev, and test.
<pre><code>
python processing/OiePreprocessor.py --batch-name train data-sample.csv sample.pk 
python processing/OiePreprocessor.py --batch-name dev data-sample.csv sample.pk
python processing/OiePreprocessor.py --batch-name test data-sample.csv sample.pk
</code></pre>

Now, your dataset with all the indexed features is in sample.pk

Training Models
------------
To train the model run the OieInduction.py file with all the required arguments:
<pre><code>
python learning/OieInduction.py --pickled_dataset sample.pk --model_name discrete-autoencoder --model AC --optimization 1 --epochs 10 --batch_size 100 --relations_number 100 --negative_samples_number 20 --l2_regularization 0.1 --alpha 0.1 --seed 2 --embed_size 30 --learning_rate 0.1
</code></pre>