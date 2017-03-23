# TextRank - *Automatic Summarization with Sentence Extraction*

### About
----
A javascript implementation of **TextRank: Bringing Order into Texts** ([PDF link to the paper](https://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf)) by Rada Mihalcea and Paul Tarau.

This only has the implementation for the **Sentence Extraction** method decsribed in the paper.

### Details
---
Given an article of text in string format it will generate a summary of the article.

###### Example case:
http://www.cnn.com/2017/03/22/opinions/puzzling-out-tsa-laptop-ban/index.html
I took all the text from the article above and put it into a string. Then ran the code below.

```javascript
// articleOfText is the string
var textRank = new TextRank(articleOfText);
console.log(textRank.summarizedArticle);
```
```
It's difficult to make sense of this as a security measure, particularly at a time when many people question the veracity of government orders, but other explanations are either unsatisfying or damning. This is why, in the past, TSA officials have demanded passengers turn their laptops on: to confirm that they're actually laptops and not laptop cases emptied of their electronics and then filled with explosives. Forcing a would-be bomber to put larger laptops in the plane's hold is a reasonable defense against this threat, because it increases the complexity of the plot. Terrorists are smart enough to put a laptop bomb in checked baggage from the Middle East to Europe and then carry it on from Europe to the US. Even more confusing, The New York Times reported that "officials called the directive an attempt to address gaps in foreign airport security, and said it was not based on any specific or credible threat of an imminent attack.
```

If you want to summarize another article you would have to create a new TextRank object again. (Thinking about changing this later)
```javascript
var textRank = new TextRank(someArticle);
console.log(textRank.summarizedArticle);

var textRank_ = new TextRank(anotherArticle);
console.log(textRank_.summarizedArticle);
```

### Settings
There are some parameters you can set in the TextRank object.

Note: You must provide both **tokens** and **split** not just one or the other if you choose you provide your own.
```javascript
var settings = { // does not compile, just pesudocode layout
    extractAmount: 6, // Extracts 6 sentences instead of the default 5.
    d: 0.95, // value from [0,1] it is the random surfer model constant default of 0.85
    sim: function(Si, Sj) { ... }, // You can use your own similarity scoring function!
    tokens: [ sentence1, sentence2, sentence3, ... , sentenceN ],
    split: [[word1, word2, ... , wordN],[word1, word2, ... , wordN], ..., [word1, word2, ... , wordN]]
}
```

#### Providing tokens yourself
Providing your own tokens means you already parsed your body of text into sentences.
In addition you must tokenize the sentences on your own and provide those. Here is an example.

```javascript
var someArticle = "Blue cats are cool. Welcome home Julie! Tacos are tasty."
var settings = {
    tokens: [ "Blue cats are cool.", "Welcome home Julie!", "Tacos are tasty."],
    split: [["blue","cats","are","cool"],["welcome","home","julie"], ["tacos","are","tasty"]]
}
// You don't actually have to provide the real someArticle text if you provide your own tokens. Just don't provide the empty string.
var textRank = new TextRank(someArticle,settings);
var textRank_same_result_as_above = new TextRank("This text does nothing!",settings);
```

Also since you provide the tokenized sentences your similarity function can vary a lot.

#### Similarity function
The parameters **Si** and **Sj** are of this format. This should help you understand what information is available to you when implementing a scoring function.
When you provide your own tokens and split, the *token* is the sentence attribute. The tokenized sentence will be *tokens* attribute.
```javascript
{
    id:0, // sentence position
    score:7.497927571481792, // sentence score (vertex score)
    sentence:"Today Judge Denise Lind announced her verdict in the case of Pfc.",
    tokens:Array[12] // looks like ["today", "judge", ... , "pfc"]
}
```
