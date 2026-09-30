# cv-rag-search

Simple local CV question answering in [cv_qa_ollama.ipynb](cv_qa_ollama.ipynb), using LangChain, Ollama, and Chroma.

The notebook loads PDF CVs, cleans and splits their text, removes duplicate chunks, creates embeddings, and retrieves relevant excerpts for the chat model to answer questions.

## Setup

Use Python 3.11 or 3.12, VS Code with the Python and Jupyter extensions, and [Ollama](https://ollama.com/).

From the project directory:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
ollama pull nomic-embed-text
ollama pull llama3.2:3b
```

Start the Ollama application, or run `ollama serve` in a separate terminal if it is not already running.

## Run

1. Open [cv_qa_ollama.ipynb](cv_qa_ollama.ipynb) and select the `.venv` kernel.
2. Set `CV_FOLDER` in the settings cell to your PDF folder. The original default is `data/cvs`, relative to the notebook working directory; these CVs are not included in the repository.
3. Restart the kernel and run the cells from top to bottom. The first cell installs everything in [requirements.txt](requirements.txt); skip it if you already installed the requirements.
4. Change the question in the final cell, for example:

```python
print(ask("Who has experience with Docker?"))
```

Use `search(question)` to inspect matching chunks. Both `search()` and `ask()` retrieve four chunks by default; pass `k=8` to retrieve more.

## Notes

- Candidate labels come from PDF filenames.
- The Chroma collection is in memory and is deleted and rebuilt each time the indexing cell runs. There is no persistent index or incremental refresh.
- This simple version has no automated evaluation, citation checking, or OCR. Use text-based PDFs and ensure the folder and Ollama models are available before running.
- Answers are prompted to use retrieved excerpts, but may still be incorrect or miss candidates. Check the original CVs; "Not found in the CVs" is not proof that a skill is absent from every CV.
- Keep private CVs out of Git and clear notebook outputs before committing. Review staged changes for personal information.

