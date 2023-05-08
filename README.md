<div id="top"></div>

<!-- PROJECT LOGO -->
<div align="center">
  <h1 align="center">Prodigy Installation and Usage Guide</h1>
</div>


<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#Requirement">Requirement</a></li>
    <li><a href="#Setup">Set up</a></li>
    <li><a href="#Installation">Installation</a></li>
    <li><a href="#Usage">NER annotation example</a></li>      
  </ol>
</details>


<!-- Requirement -->
## Requirement 
The prodigy requires Python 3.6 and above. Run the following commands if you want to enable the openai auto-annotation.

```bash 
python -m pip install -r requirements.txt
```

<!-- Setup -->
## Setup
Use following commands to create a virtual environment. 
```base 
conda create --name prodigy python=3.9
conda activate prodigy
```

<!-- Installation -->
## Installation 
Run the following commands to install prodigy.  
```bash 
pip install prodigy -f https://[license key]@download.prodi.gy
```

<!-- Usage -->
## Usage 
#### Use case 1: NER annotation from scratch example

Start an annotation job: 
```bash
prodigy ner.manual ner_news_headlines blank:en ./news_headlines.jsonl --label PERSON,ORG,PRODUCT,LOCATION
```

Components in the above command:

<ul>
    <li> ner.manual: prodigy built-in recipes, check https://prodi.gy/docs/recipes#ner-manual for more selections and details. </li>
    <li> ner_news_headlines: database to store the annotation results. </li>
    <li> blank:en: the spacy pipeline or language to tokenize the text (blankLen for a blank English model, more on that later) </li>
     <li> ./news_headlines.jsonl: path to the text you want to label. </li>
     <li> --label PERSON,ORG,PRODUCT,LOCATION: comma-separated list of label options (PERSON, ORG, PRODUCT AND LOCATION) </li>
</ul>

Check annotation data 
```bash 
prodigy db-out ner_news_headlines > ./annotations.jsonl
```

#### Use case 2: NER annotation with OpenAI as auto-annotator
```bash
prodigy ner.openai.correct ner_correct_test data/reddit_r_cooking_sample.jsonl "ingredient,dish,equipment" -F recipes/openai_ner.py
```
Components in the above command:

<ul>
    <li> ner.openai.correct: a new defined recipt associated with the recipets following -F. </li>
    <li> -F recipts/openai_ner.py a python file to suggest annotation. </li>
</ul>


## Properly shut down the annotation
To close the annotation, we need to use 'Ctrl' + 'C' every time you finish the annotation task. 

If an error message "ERROR:    [Errno 48] error while attempting to bind on address ('::1', 8080, 0, 0): address already in use" 
shown, run below command to list working session.

```bash 
sudo lsof -i :8080
```

And kill the previous session using 
```
kill -9 {PID}
```