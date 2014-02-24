#PAN-2012 IRC corpus notes

- Participants received separate training and testing sets.
- Conversations break at no response in 25 minutes
- 77% - 99% of chat conversations are less than 150 messages. Therefore all of the conversations in the corpuses are less than 150 messages.
- 1% or less conversations involve a sexual predator (true positive)
- True positive = sexual predator, false positive = sexual sounding non-predator, false negative = general conversation
- Lexical features and behavioral features
- Lexical features are those that can be derived from the raw text of the conversation: example of these features are unigram or bigram, their weighting using TF-IDF or the cosine similarity and emoticons counting. It is to be noted that, in general, lexical features have been used without stemming or stop word removal, to preserve each author's own style, including misspelling and grammatical errors.
- Behavioral features are those that capture the "actions" of a user within a conversation: the number of times a user starts a dialogue, the response time after a message of the partner in the conversation, the number of questions asked, the frequency of turn-taking, intention (grooming, hooking, ...), etc. 
- Language Model (LM) was built for each author.
- Support Vector Machines were the most used classification methods. Neural Network classifier was also successful. Others tried were Maximum-Entropy, decision trees, k-NN, and/or random forest as well as Naive Bayes.- What is Precision vs Recall?
- Using a pre-filtered step to prune irrelevant conversations seems an important addition to the system.