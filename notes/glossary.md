### Natural Language Processing (NLP)
Making machines understand or produce natural language e.g. English, Spanish, Arabic. Example applications of NLP are speech interfaces (Alexa, Siri), search, and sentiment analysis.

### corpus

Collection of text, usually by a particular author or theme. May or may not be annotated by a human. Some examples can be found at [nlp datasets](https://github.com/niderhoff/nlp-datasets).

### tokenization

Breaking up text into smaller pieces, called _tokens_. Tokens may or may not be words. There is not one specific way to perform tokenization, for example the sentence "Send questions to my e-mail foo@bar.baz" could be tokenized multiple ways such as: [send, questions, to, my, e, mail, foo, bar, baz] or [send, questions, to, my, e-mail, foo@bar.baz] depending on how you define your tokenizer.

### stopwords

List of words/tokens to filter out before analysis. Typically common words such as the, what, a, etc...

### Part-of-Speech (POS) tagging

Tagging words with their part of speech, such as noun, verb, or adjective. Since words can change part of speech depending on context this process can be error-prone and complex. Taggers are typically trained machine learning models. 

### stemming

Combining variations of a word by chopping off the end of the word. For example, Porter's algorithm would convert `ponies` -> `poni` and `cats` -> `cat`. Stemming is fast and gives a high number of relevant results, but can have a high false positive rate.

### Lemmatization

Grouping words into their *lemma*, or standard form of the word. For example, `better` and `best` have the lemma `good` whereas `walk` is the lemma for `walking`, `walked`, and `walks`. Lemmatization is more complicated than stemming, but has fewer false positives. 

### Named Entity Recognition (NER)

Classifying words or phrases of text into categories such as people, organizations, time, or disease. 

### NLP Pipeline

A series of NLP processes performed on a text in succession. For example, to index a document for searching a basic NLP pipeline would be:

1. Tokenize
2. Filter out stopwords
3. Lemmatization

For searching you typically want to put user queries and the documents to index through the same NLP pipeline. 


