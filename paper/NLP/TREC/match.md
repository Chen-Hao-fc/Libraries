In order to get all the passages associated with the query. We use multi-way matching strategies including string- based, corpus- based and semantic-based methods. The string-based method operates on character composition that measures similarity  between query and passage for approximate string matching or comparison. Compare to string-based method at the literal level, the corpus-based method uses the information obtained from the corpus to calculate the text similarity. Further more, the semantic based method can match the semantic correlation of query and passage. It by generating word vectors through meachine learning methods, low-dimensional real vectors can be trained in unstructured text with no mark, which makes similar words closer in distance.

String-based

 We use N-gram method compare the n-grams $g_n$ between a query and a passage. Specifically, for a query and passage pairs $(p_i， q_i)$, we divide $q_i$ into several n-grams(consecutive word) with the length of n, and to determine whether $g_n$ is included in $p_i$, a match scoring function could be written as:
$$
S(p_i, q_i)= \exp(\sum_{n=2}^4 w_n\log p_n)\\
p_n=\frac{\sum_{g_n\in q_i\bigcap g_n\in p_i}Count_{dist}(g_n)}{\sum_{g_n\in q_i}Count(g_n)}
$$
Having consecutive words at the same time usually has higher similarity, so we discard a single word and only consider the case of consecutive words with n being 2, 3, and 4. Where $Count_{dist}$ means that the same n-gram is calculated only once.,and  $w_n$ is the weight of n-gram, we employ uniform weights $w_n=1/3$.

Corpus-base



 

