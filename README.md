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
    <li><a href="#Example">NER annotation example</a></li>      
  </ol>
</details>


<!-- Requirement -->
## Requirement 
The prodigy requires Python 3.6 and above. 

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

<!-- Example -->
## NER annotation example

Start an annotation job: 
```bash
prodigy ner.manual ner_news_headlines blank:en ./news_headlines.jsonl --label PERSON,ORG,PRODUCT,LOCATION
```

Components in the above command:

<ul>
    <li> ner.manual: prodigy built-in recipes, check [here](https://prodi.gy/docs/recipes#ner-manual) for more selections and details. </li>
    <li> ner_news_headlines: database to store the annotation results. </li>
    <li> blank:en: the spacy pipeline or language to tokenize the text (blankLen for a blank English model, more on that later) </li>
     <li> ./news_headlines.jsonl: path to the text you want to label. </li>
     <li> --label PERSON,ORG,PRODUCT,LOCATION: comma-separated list of label options (PERSON, ORG, PRODUCT AND LOCATION) </li>
</ul>

Check annotation data 
```bash 
prodigy db-out ner_news_headlines > ./annotations.jsonl
```

