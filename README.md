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
prodigy ner.openai.correct ner_correct_test data/jd_data.jsonl "skill" -F recipes/openai_ner.py -p templates/ner_prompt.jinja2 -e examples/skill_ner.yaml
```
Components in the above command:

<ul>
    <li> ner.openai.correct: a new defined recipt associated with the recipets following -F. </li>
    <li> -F recipts/openai_ner.py a python file to suggest annotation. </li>
</ul>

More details and other parameters are listed in table below: 

| Argument                | Type | Description                                                                                                                                     | Default                         |
| ----------------------- | ---- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| `dataset`               | str  | Prodigy dataset to save annotations to.                                                                                                         |                                 |
| `filepath`              | Path | Path to `.jsonl` data to annotate. The data should at least contain a `"text"` field.                                                           |                                 |
| `labels`                | str  | Comma-separated list defining the NER labels the model should predict.                                                                          |                                 |
| `--lang`, `-l`          | str  | Language of the input data - will be used to obtain a relevant tokenizer.                                                                       | `"en"`                          |
| `--segment`, `-S`       | bool | Flag to set when examples should be split into sentences. By default, the full input article is shown.                                          | `False`                         |
| `--model`, `-m`         | str  | GPT-3 model to use for initial predictions.                                                                                                     | `"text-davinci-003"`            |
| `--prompt_path`, `-p`   | Path | Path to the `.jinja2` [prompt template](templates).                                                                                             | `./templates/ner_prompt.jinja2` |
| `--examples-path`, `-e` | Path | Path to examples to help define the task. The file can be a .yml, .yaml or .json. If set to `None`, zero-shot learning is applied.              | `None`                          |
| `--max-examples`, `-n`  | int  | Max number of examples to include in the prompt to OpenAI. If set to 0, zero-shot learning is always applied, even when examples are available. | 2                               |
| `--batch-size`, `-b`    | int  | Batch size of queries to send to the OpenAI API.                                                                                                | 10                              |
| `--verbose`, `-v`       | bool | Flag to print extra information to the terminal.                                                                                                | `False`                         |


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

Notice that the templates/Kor_format.jinja2 is not working at this point. 