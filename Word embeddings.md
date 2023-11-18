Word embeddings are [[Embedding]] operations that turn words into [[Latent space]] vectors. There are different ways of training them.
	word2vec:
		create two different vectors for each word in the vocabulary, initiated with random values for their components, $(V_{ij},U_{ij})$ $V_{ij}$ is the jth component of one of the two vectors of the ith word, and $U_{ij}$ is the same for the other vector. The objective is to optimize the dot products between word vectors to closly resemble the co-ocurrence matrix ($M_{ij}$) $$
		\mu_{ij} = softmax_U(U^TV)
		$$
		probability of 
		$$
		P(i|j) = \frac {\exp(\vec{U}_i \cdot \vec{V}_j)}{\sum \limits _{k = 0}^{vocab} \exp(\vec{U}_k \cdot \vec{V}_j)}
	$$
		 