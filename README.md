[![Built with askThili](https://msrit.ask.thili.ai/api/v1/public/projects/d4rewxuerp/badge.svg)](https://msrit.ask.thili.ai/b/d4rewxuerp)
# Medical Terminology Helper
An offline command-line study tool for introductory anatomy. Type a phrase such as "Arteries carry" to generate its continuation. The bundled model learns a small set of study sentences. Unfamiliar wording can produce inaccurate or repetitive text. For vocabulary practice only.

## Run on Windows
From the repository root with Python 3.12 installed:
```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m app.cli "The right ventricle"
.\.venv\Scripts\python.exe -m app.cli
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```
The one-shot example prints "The right ventricle pumps blood to the lungs." Interactive mode exits on an empty line. Other practice prompts include "Bone marrow", "The left atrium", and "Homeostasis maintains".

## How it works
1. Each line of data/phrases.txt is a study example. data/SOURCES.md documents the source texts.
2. app/tokenizer.py learns byte-pair merges within words and uses separate start and end tokens.
3. app/model.py adds learned position and token embeddings and applies masked single-head attention, residual connections, normalization and a feed-forward network. NumPy performs the matrix operations.
4. app/train.py trains all weights with masked next-token cross-entropy and Adam. Training uses a fixed seed and 500 minibatch updates. To reproduce the checkpoint run `python -m app.train` in the installed environment.
5. app/generate.py loads the saved vocabulary and weights, repeatedly selects the highest-scoring next token and stops at a period, end token or length limit.
6. app/cli.py provides one-shot and interactive practice. GitHub Actions runs the tests on Python 3.12.

The 24-sentence corpus focuses on recall. The checkpoint tests check learned completions and do not measure general medical knowledge. No API key, model download or server is required after installing NumPy.
