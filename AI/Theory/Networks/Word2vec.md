A popular [[Word embeddings]].
#### Concepts
**corpus**: Dataset of ordered words (books, Wikipedia, …) on which we try to predict word similarity based on close co-occurrence.
	"_A machine learning algorithm is an algorithm whose behavior depends on numerical parameters that can be optimized according to a performance metric. Just like for regular algorithms, functions (relationships between inputs and outputs) can be very important for machine learning algorithms, this is why neural networks are very commonly used, since they are universal function aproximators._"
**vocabulary**: All the words the embedding will turn into vectors, it comes from all the unique words in the corpus.

|words|
|---|
|A|
|Just|
|a|
|according|
|algorithm|
|algorithms|
|an|
|and|
|aproximators|
|are|
|be|
|behavior|
|between|
|can|
|commonly|
|depends|
|for|
|function|
|functions|
|important|
|inputs|
|is|
|learning|
|like|
|machine|
|metric|
|networks|
|neural|
|numerical|
|on|
|optimized|
|outputs|
|parameters|
|performance|
|regular|
|relationships|
|since|
|that|
|they|
|this|
|to|
|universal|
|used|
|very|
|whose|
|why|

**center word**: While running through the corpus we focus on one word and its context and then continue to the next, the word we are focusing on is the center word. its position is named "t" in the math 
	"_A machine learning algorithm is an algorithm whose behavior depends on numerical parameters that can be optimized according to a performance metric. Just like for regular algorithms, functions (relationships between inputs and outputs) can be very ==important== for machine learning algorithms, this is why neural networks are very commonly used, since they are universal function approximators._"
**window**: The amount of words left and right that we consider part of the context. It's size is named "c" in the math.
	"_A machine learning algorithm is an algorithm whose behavior depends on numerical parameters that can be optimized according to a performance metric. Just like for regular algorithms, functions (relationships between inputs and outputs) ==can be very important for machine learning== algorithms, this is why neural networks are very commonly used, since they are universal function approximators._"

Create two different vectors for each word in the vocabulary (equivalent to OneHotEncoding times a matrix of word embeddings), initiated with random values for their components, $(V_{ij},U_{ij})$ $V_{ij}$ is the jth component of one of the two vectors of the ith word, and $U_{ij}$ is the same for the other vector. The objective is to optimize the dot products between word vectors to closly resemble the co-ocurrence matrix ($M_{ij}$) $$
\mu_{ij} = softmax_U(U^TV)
$$
prediction of probability of word i appearing given word j using their embedded vectors. 
$$
\hat p(i|j) = \frac {\exp(\vec{U}_i \cdot \vec{V}_j)}{\sum \limits _{k = 0}^{vocab} \exp(\vec{U}_k \cdot \vec{V}_j)}
$$

When the algorithm is in this window:
	"_can be very ==important== for machine learning_"
It predicts with the center word the probability of finding any word in the vocabulary:
$$
\hat p(i|important) = \frac {\exp(\vec{U}_i \cdot \vec{V}_{important})}{\sum \limits _{k = 0}^{vocab} \exp(\vec{U}_k \cdot \vec{V}_{important})}
$$
And we compare that distribution with each one hot encoding of the words in the context
$$
compare(\hat p(i|important), OHE(cat)) = 
compare(\hat y,y)
$$
the comparison function is the [[KL divergence]] which in this case it simplifies to
$$
-\sum y_i log(\hat y_i) =  
$$
