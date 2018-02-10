<sup>Find the Jupyter notebooks for these posts on [my Github](https://github.com/ikottman/learning-nlp)</sup>

Welcome to the first in a series of posts chronicling my endeavors to teach myself natural language processing. I'll be focusing on the practical side of NLP, going through common techniques with examples in the form of Jupyter notebooks. Most of these will use the Python library [spaCy](https://spacy.io/).

First, we'll start off with the basic components of a language processing pipeline.

## Tokenization

Tokenization is the process of breaking up text into smaller pieces, called _tokens_. Tokens may or may not be words. There is not one specific way to perform tokenization, as different problems may call for different granluarity of tokens. Below are two examples of tokenizers.


```python
text = "I'm against picketing, but I don't know how to show it."
```


```python
# A naïve tokenizer that splits on spaces.
tokens = text.split(" ")
print(tokens)
```

    ["I'm", 'against', 'picketing,', 'but', 'I', "don't", 'know', 'how', 'to', 'show', 'it.']


Just splitting on spaces gets you 80% of the way there, but it doesn't take into account things like punctuation.


```python
# The default tokenizer in spaCy is a bit more robust.
import spacy
from spacy.lang.en import English

english = spacy.load('en')
tokenizer = English().Defaults.create_tokenizer(english)

tokens = tokenizer(text)
tokens = [token.text for token in tokens]

print(tokens)
```

    ['I', "'m", 'against', 'picketing', ',', 'but', 'I', 'do', "n't", 'know', 'how', 'to', 'show', 'it', '.']


It treats words and symbols as separate tokens, and even splits up contractions.

## Stopwords

Stop words are merely words you want your NLP pipeline to ignore. Stop lists usually contain common words like 'the', but they may also contain domain-specific words like 'Cerner' or 'DevCon'.


```python
from spacy.lang.en import English

stopwords = list(English.Defaults.stop_words)

print(f"Number of stopwords in spaCy: {len(stopwords)}")
print(f"Examples:\n{' '.join(stopwords[0:10])}")
```

    Number of stopwords in spaCy: 305
    Examples:
    or had down take thereupon when has fifty the still



```python
text = "I'm against picketing, but I don't know how to show it."

english = spacy.load('en')
doc = english(text)

# spaCy automatically tags when words are stop words with `is_stop`
tokens = [token for token in doc if not token.is_stop]

print(tokens)
```

    [I, 'm, picketing, ,, I, n't, know, .]

# Combining Similar Words

Two common ways of combining similar words are _stemming_ and _lemmatization_.

## Stemming

The simpler of the two, and works by chopping off the end of each word. A popular algorithm for this, called Porter's algorithm, would convert `ponies` -> `poni` and `cats` -> `cat`. Stemming is fast but can have a high false positive rate.


```python
from nltk.stem.porter import PorterStemmer
stemmer = PorterStemmer()

text = "This shirt is dry-clean only which means it's dirty"
tokens = text.split(" ")
singles = [stemmer.stem(token) for token in tokens]
for token in tokens:
    stem = stemmer.stem(token)
    print(stem)
```

    thi
    shirt
    is
    dry-clean
    onli
    which
    mean
    it'
    dirti


Stemming doesn't take into account parts of speech, and can produce invalid words. This is why spaCy doesn't include a stemmer, instead it provides lemmatization.

## Lemmatization
Grouping words into their _lemma_, or standard form of the word. For example, `better` and `best` have the lemma `good` whereas `walk` is the lemma for `walking`, `walked`, and `walks`. Lemmatization is more complicated than stemming, but has fewer false positives.


```python
import spacy
from spacy.lang.en import English

text = "I haven't slept for ten days, because that would be too long."

nlp = spacy.load('en')
doc = nlp(text)
for token in doc:
    if not token.is_punct and token.text != token.lemma_:
        print(token.text, "->", token.lemma_)
```

    I -> -PRON-
    n't -> not
    slept -> sleep
    days -> day


You can see plurals and different tenses are collapsed into a single base form. spaCy also introduces a special case for pronouns because there is no clear lemma for pronouns. Should "I" become "me", "it", or "they"? To avoid this ambiguity they treat all pronouns as a special lemma, -PRON-.
