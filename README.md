

# Practical NLP Study Project

Practical NLP for Survey Analysis with deepsight GmbH at Universität Osnabrück.

### Overview

This repository hosts the code for the study project 'Practical NLP', an interdisciplinary study project at Osnabrück University held during WS2019-SS2020. The project implements a topic analysis pipeline including data cleaning, sentiment analysis, and document clustering, optimized to employee survey data. For further information on the project scope, individual modules, and methods employed, please refer to the online [documentation](https://pnlpuos.github.io/).

***

### Project Structure

<pre>
|-- data
|-- notebooks
    |-- sentiment_analysis
    |-- topic_clustering
    |-- data_prep
|-- src
    |-- sentiment_classifier
    |-- topic_modeling
    |-- outputs
</pre>


***

### Installation

1. Set up your virtual environment. Because the package requires several third-party dependencies and models, it is recommended that you instantiate a new virtual environment. Please also ensure that you have approx. 4GB of available drive space. Ensure your environment has Cython installed.
2. Install PyTorch from [source](https://pytorch.org/). The package is tested on Python==3.7.4 with pytorch==1.6.0. Please ensure that your Python installation matches your system (32 or 64bit).
3. Clone the repository.

<pre>$ git clone https://github.com/PNLPUOS/PNLPUOS.git</pre>

4. Navigate to the cloned directory and install with pip.

<pre>$ pip install .</pre>

5. Note: OS X users require a compiler with good C++11 support per the [FastText documentation](https://fasttext.cc/docs/en/support.html). Information on how to install one of the available compilers can be found [here](https://www.ics.uci.edu/~pattis/common/handouts/macclion/clang.html).
6. Obtain fasttext English model [here](https://fasttext.cc/docs/en/english-vectors.html). Place ‘common-crawl-300d-2M-subword’ in 'pnlp' directory. This model is only required if you wish to use the non-default FastText embeddings for topic modeling and labeling.

### Usage

1. Obtain a dataset for analysis. The pipeline is optimized to employee survey comments in .csv  (semicolon-delineated) conforming to the following format:

| Report Grouping | Question Text     | Comments   |
| --------------- | ----------------- | --------- |
| Department 1    | Survey Question 1 | Comment 1 |
| Department 2    | Survey Question 2 | Comment 2 |
| ...             | ...               | ...       |
| Department n    | Survey Question n | Comment n |

If your data does not contain distinct questions or report grouping attributes, then do not include these columns in your dataset. The pipeline will then perform an attribute-agnostic analysis on the comments alone. Currently, the pipeline only supports analysis of English data.

2. Run the main analysis pipeline on your dataset by passing the filepath as a command argument.

<pre>$ python -m pnlp --path yourfilepath</pre>

This command will run a default analysis pipeline, outputting several log files and summary analytics including visualization of identified document clusters and sentiment labels.

#### Module-Specific Functions

You may run module-specific functions independently by calling them from their respective modules. Some examples are provided below.

**Transformer-Based Contextual Spell Correction**

```python
from pnlp.src.spell_correction import transspell

ts = transspell.TransSpell()
sentence = 'This sentnce contains an error.'
print(ts.correct_errors(sentence))
>>> This sentence contains an error.
```

**Sentiment Prediction**

```python
from pnlp.src.sentiment_classifier.rnn_sentiment import predict_sentiment
import pandas as pd

docs = pd.Series(['I love this.', 'I do not care.'])
predictions = predict_sentiment(docs, load='question1')
print(predictions.values)
>>> [1 0] # positive, neutral
```

**Preprocessing**

```python
from pnlp.src.utilities import preprocessing
import pandas as pd

docs = ['This is a document with several stopwords.',
        'This!!.; is a document,,, with lots'' of && punctuation!?']
docs = pd.Series(docs)
docs = preprocessing(docs, tokenize=True, stopwords=True, punctuation=True)
print(docs.values)
>>> [list(['document', 'several', 'stopwords'])
     list(['document', 'lots', 'punctuation'])]
```

***

### Optional Arguments

You may pass several optional arguments to override the default pipeline configuration. To view these arguments please consult the output of the following command.

<pre>$ python -m pnlp --help</pre>

***


