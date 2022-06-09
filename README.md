# Xing2019

## abstract

Sentiment lexicons are essential tools for polarity classification and opinion mining. In contrast to machine learning methods that only leverage text features or raw text for sentiment analysis, methods that use sentiment lexicons embrace higher interpretability.

Although a number of domain-specific sentiment lexicons are made available, it is impractical to build an ex ante lexicon that fully reflects the characteristics of the language usage in endless domains.

In this article, we propose a novel approach to simultaneously train a vanilla sentiment classifier and adapt word polarities to the target domain. Specifically, we sequentially track the wrongly predicted sentences and use them as the supervision instead of addressing the gold standard as a whole to emulate the life-long cognitive process of lexicon learning.An exploration-exploitation mechanism is designed to trade off between searching for new sentiment words and updating the polarity score of one word.

Experimental results on several popular datasets show that our approach significantly improves the sentiment classification performance for a variety of domains by means of improving the quality of sentiment lexicons. Case-studies also illustrate how polarity scores of the same words are discovered for different domains.

## Introduction

<b>Domain adaptation (Blitzer, Dredze, & Pereira, 2007)</b> has been identified as a key issue in sentiment analysis and its applications, such as customer opinion mining (Hu & Liu, 2004), human-computer interaction (Shi & Yu, 2018), and business intelligence (Xing, Cambria, & Welsch, 2018b). This is because that many topics we discuss are characterized by their own sub-languages, such as special terminologies and jargons.

Being unfamiliar with these topics (or domains), e.g., food, finance, movie, sports etc. will lead to misunderstanding of the sentiment conveyed. For the same reason, we observe that the performances of sentiment analysis usually drop when using sentiment lexicons of the general domain or other irrelevant domains. Therefore, direct use of lexical resources is often suboptimal (Choi & Cardie, 2009). Domain adaptation is a necessary procedure to cast the general domain sentiment lexicon or sentiment classifier for practical use.

Although direct adaptation of the sentiment classifier is popular in recent studies, many critical tasks require their model to be equipped with a sentiment lexicon (Taboada, Brooke, Tofiloski, Voll, & Stede, 2011). These tasks include applications that prefer high interpretability, such as medicine, healthcare, and financial forecasting (Denecke & Deng, 2015; Xing, Cambria, Malandri, & Vercellis, 2018a).

However, a frustrating fact is that the attempt to create an ex ante lexicon for every domain is Sisyphean: there are endless domains. For instance, the electronic product domain has sub-domains, e.g., camera and phone. As a result, supervision is still preferred to precisely define a language domain.

Whereas word-level supervision is hardly accessible because word polarities are considered as the latent information. And the task of deriving a domain-specific sentiment lexicon is trivial if we have direct access to the word-level supervision. On the contrary, language resources for sentiment analysis are usually from social media (Mohammad, Zhu, Kiritchenko, & Martin, 2015) or rating websites (Hung, 2017), where high-level supervisions are provided by users.

Therefore, how to leverage high-level (e.g., expression-level, sentence-level, or even document-level) supervision becomes an intriguing question.

In this article, we propose a novel approach that explicitly presents a sentiment lexicon and expects all the polarity scores to change during the learning phase.Like what word embedding (Mikolov, Sutskever, Chen, Corrado, & Dean, 2013) is to the neural language model (Bengio, Ducharme, & Vincent, 2003), the adapted sentiment lexicon is a by-product, yet an important one as the algorithm gradually learns from the high-level supervision.

We consider our approach as cognitive-inspired in a sense that the algorithm emulates several metacognition processes when a human agent is exposed to a new language domain: the agent has presumptions of word polarities according to his/her prior knowledge (Pinker, 2004); his/her lexical knowledge would not change before a conflict is detected; given the conflict, he/she would try to locate a word as the hypothetical cause of the misunderstanding and apply an alternative understanding, which is subject to further confirmation or disproval in future occasions when the word show up again.

We develop several heuristic rules, such as the exploration-exploitation trade-off and some cognitive constraints to model this process. Fig. 1 depicts the fundamental ideas of our approach. The first record is predicted as negative because dump and loss are negative words in the original lexicon.

However, user labeled this record as positive. From the label our approach identifies neutral word small as the cause of this conflict and learns a positive polarity of word small. In fact, from the perspective of aspect-based or concept-level sentiment analysis, small loss is a positive phrase. However, this knowledge of word small is revised by the second record where thread small is a negative phrase.

This negative polarity of word small can be used to predict the sentiment of the third record as negative, which is consistent with the supervision. Although the word that really carries a negative sentiment is cracking, all the polarity scores keep unchanged in this situation. In the long run, we could expect the discovery of true word sentiment as training samples continuously arrive. In the following sections, we conduct extensive experiments to demonstrate the effectiveness of our approach.

The remainder of the article is organized as follows. Section 2 reviews existing approaches to the problem of domain adaptation. Section 3 introduces several heuristic rules used in our approach and presents a domain adaptation algorithm. Experimental results are discussed in Section 4. Section 5 concludes the paper.
