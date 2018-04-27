# Mianbot
---

![demo](https://raw.githubusercontent.com/zake7749/Chatbot/master/docs/demo.png)

Mianbot is a chat robot built using a model and search model. There are currently two ways to generate a reply. The project is still under development:)

* The first (left) is the phrase classification based on the word vector, and the feature extraction and memory reply function is implemented for the target module of the classification to perform multiple rounds of dialogues. The matching method can refer to the [Semantic Graph](https://github.com/zake7749/Semantic-Graph) (currently still under construction).
* The second (right) except the weather response, mainly based on PTT Gossiping as the knowledge base, through the comparison of text similarity to retrieve the article title most similar to the user input, and then pick the most reliable reply from the set of tweets. Program content and experimental process, please refer to [PTT-Chat_Generator](https://github.com/zake7749/PTT-Chat-Generator).

## Matching Example
---

More examples can refer to `example/output.txt`

Enter：Call me up tomorrow morning.

|Similarity|Concept|Matching element|
|------|----|------|
|0.4521|Alarm clock|get up|
0.3904|the weather|morning|
0.3067|stay|get up|
0.1747|disease|get up|
0.1580|buy|morning|
0.1270|stock|morning|
0.1096|go sightseeing|morning|

Input: Will Shanghai be raining tomorrow?

Similarity|concept|Matching element|
|------|----|------|
0.5665|the weather|rain|
0.3918|Alarm clock|rain|
0.1807|disease|rain|
0.1362|stay|rain|
0.0000|stock| |
0.0000|go sightseeing| |
0.0000|buy| |

## Environmental requirements
---

* Install python3 development environment
* Install [gensim – Topic Modelling in Python](https://github.com/RaRe-Technologies/gensim)

```python
import Chatbot.console as console
c = console.Console(model_path='your_model')
```

* To use the QA module, first in accordance [with the test data set Q](https://github.com/zake7749/Chatbot#%E5%95%8F%E7%AD%94%E6%B8%AC%E8%A9%A6%E7%94%A8%E8%B3%87%E6%96%99%E9%9B%86) configuration, or through the`chatbot.py` the `self.github_qa_unupdated` set `True` off QA module selection.

## How to use
---

### Chat robot

```python
import Chatbot.chatbot as chatbot

chatter = chatbot.Chatbot(w2v_model_path='your_model')
chatter.waiting_loop()
```

### Calculate the degree of match

```python
import Chatbot.console as console

c = console.Console(model_path='your_model')
speech = input('Input a sentence:')
res,path = c.rule_match(speech)
c.write_output(speech,res,path)
```

## Rule format

Json format using rules, the rule is placed in a model `\RuleMatcher\rule` in，

```json
    {
        "domain": "The abstract concept representing this rule",
        "response": [
		    "corresponds to this rule",
        	"the reply the robot will give",
        	"the robot will randomly pick a response"
        ],
        "concepts": [
            "The Possible Way of Representing the Rule"
        ],
        "children": ["The rules of the rule","If you buy -> Buy drinks, buy clothes..."]
    }
```

### Example

```json
    {
        "domain": "Purchasing",
        "response": [
        	"Pushing you to shopping module"
        ],
        "concepts": [
            "Purchasing", "Shopping", "Ordering"
        ],
        "children": [
            "buy daily necessities",
            "buy home appliances",
            "buy food",
            "buy drinks",
            "buy shoes",
            "buy clothes",
            "buy computer products."
        ]
    },
```
## Question and answer test data set

Please click [here to](https://drive.google.com/file/d/0BxfXm7KkNKc-RkY2Z1pONUlqODg/view?usp=sharing) (**# need to link to english dataset**) download some of the test datasets, including PTT C_Chat and Gossiping non-news quiz about 250,000. After unzipping the file, place the contents under `QuestionAnswering/data/`. Then move the unzipped folder `reply.rar` under `QuestionAnswering/data/processed`. The resulting folder structure should look like this:
```
QuestionAnswering
└── data
   ├── SegTitles.txt
   ├── processed
   │   └── reply
   │       ├── 0.json
   │       ├── .
   │       ├── .
   │       ├── .
   │       └── xxx.json
   └── Titles.txt
```
Once configured, you can change the `self.github_qa_unupdated` configuration in `chatbot.py` to `True`. This will open the Q & A module for testing.

## Development log
* [Word-based topic matching](http://zake7749.github.io/2016/08/30/chatterbot-with-word2vec/)
* [Development idea of the chat robot](http://zake7749.github.io/2016/12/17/how-to-develop-chatbot/)

## Acknowledgement

This is a translated version of the original [Chatbot](https://github.com/zake7749/Chatbot).
