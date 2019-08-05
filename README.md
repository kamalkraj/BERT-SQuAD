# BERT-SQuAD

Use google BERT to do SQuAD  !


# What is SQuAD?
Stanford Question Answering Dataset (SQuAD) is a reading comprehension dataset, consisting of questions posed by crowdworkers on a set of Wikipedia articles, where the answer to every question is a segment of text, or span, from the corresponding reading passage, or the question might be unanswerable.

# Requirements
- python3
- pip3 install -r requirements.txt

# Result
`model` : bert-large-uncased-whole-word-masking 
```json
{"exact_match": 86.91579943235573, "f1": 93.1532499015869}
```
## Pretrained model download from [here](https://www.dropbox.com/s/8jnulb2l4v7ikir/model.zip)
unzip and move files to model directory

# Inference
```python
from bert import QA

model = QA('model')

doc = "Victoria has a written constitution enacted in 1975, but based on the 1855 colonial constitution, passed by the United Kingdom Parliament as the Victoria Constitution Act 1855, which establishes the Parliament as the state's law-making body for matters coming under state responsibility. The Victorian Constitution can be amended by the Parliament of Victoria, except for certain 'entrenched' provisions that require either an absolute majority in both houses, a three-fifths majority in both houses, or the approval of the Victorian people in a referendum, depending on the provision."

q = 'When did Victoria enact its constitution?'

answer = model.predict(doc,q)

print(answer['answer'])
# 1975
print(ans.keys())
# dict_keys(['answer', 'start', 'end', 'confidence', 'document']))
```
`model.predict(doc,q)` return `dict`
```json
{
"answer" : "answer text",
"start" : "start index",
"end" : "end index",
"confiednce" : "confidence of answer",
"document" : "tokenzied document , list"
}
```

# Deploy REST-API
BERT QA model deployed as rest api

```bash
python api.py
```
API will be live at `0.0.0.0:8000` endpoint `predict`

### cURL request
```bash
curl -X POST http://0.0.0.0:8000/predict -H 'Content-Type: application/json' -d '{ "document": "Victoria has a written constitution enacted in 1975, but based on the 1855 colonial constitution, passed by the United Kingdom Parliament as the Victoria Constitution Act 1855, which establishes the Parliament as the states law-making body for matters coming under state responsibility. The Victorian Constitution can be amended by the Parliament of Victoria, except for certain 'entrenched' provisions that require either an absolute majority in both houses, a three-fifths majority in both houses, or the approval of the Victorian people in a referendum, depending on the provision.","question":"When did Victoria enact its constitution?" }'
```
#### Output
```json
{
    "result": {
        "answer": "1975",
        "confidence": 0.940746070672879,
        "document": [
            "Victoria",
            "has",
            "a",
            "written",
            "constitution",
            "enacted",
            "in",
            "1975,",
            "but",
            "based",
            "on",
            "the",
            "1855",
            "colonial",
            "constitution,",
            "passed",
            "by",
            "the",
            "United",
            "Kingdom",
            "Parliament",
            "as",
            "the",
            "Victoria",
            "Constitution",
            "Act",
            "1855,",
            "which",
            "establishes",
            "the",
            "Parliament",
            "as",
            "the",
            "states",
            "law-making",
            "body",
            "for",
            "matters",
            "coming",
            "under",
            "state",
            "responsibility.",
            "The",
            "Victorian",
            "Constitution",
            "can",
            "be",
            "amended",
            "by",
            "the",
            "Parliament",
            "of",
            "Victoria,",
            "except",
            "for",
            "certain",
            "entrenched",
            "provisions",
            "that",
            "require",
            "either",
            "an",
            "absolute",
            "majority",
            "in",
            "both",
            "houses,",
            "a",
            "three-fifths",
            "majority",
            "in",
            "both",
            "houses,",
            "or",
            "the",
            "approval",
            "of",
            "the",
            "Victorian",
            "people",
            "in",
            "a",
            "referendum,",
            "depending",
            "on",
            "the",
            "provision."
        ],
        "end": 7,
        "start": 7
    }
}
```
#### Postman
![postman output image](/img/postman.png)
