# Mr.Wiki
[en](README.md) | [简体中文](docs/README_zh.md)

<p align="center">
  <img src="https://github.com/QSCTech-Sange/Mr.Wiki/blob/main/docs/sample.gif">
</p>

Mr.Wiki is a RAG retrieval chatbot based on the local LLM (Large Language Model) and utilizes a vector database. It is developed using Python3 and incorporates the following components:

- [streamlit](https://streamlit.io/) for frontend interface construction
- [llamaindex](https://www.llamaindex.ai/) LLM data framework
- [chromadb](https://www.trychroma.com/) for vector database

Key features include:

- Small and simple (only one Python3 file with less than 160 lines)
- User-friendly web interface
- Direct manipulation of the vector database on the interface, allowing document addition, deletion, and selection for queries 
- Persistence of the vector database locally

# Installation
Use the following commands to download the repository from GitHub, create a Python virtual environment, and install dependencies.
```shell
git clone git@github.com:QSCTech-Sange/Mr.Wiki.git
cd Mr.Wiki && mkdir pyenv
cd pyenv && python3 -m venv . 
cd bin && source activate
cd ../.. && pip3 install -r requirements.txt
```
If you are sure you don't need a virtual environment, you can directly use:
```shell
git clone git@github.com:QSCTech-Sange/Mr.Wiki.git
cd Mr.Wiki
pip3 install -r requirements.txt
```
# Usage
1. Modify the code to use the desired large language model:
```python
llm = OpenAI(api_base="http://localhost:1234/v1", api_key="not-needed")
```
In this example, the code uses LM Studio with the model qwen1_5-14b-chat-q3_k_m.gguf mounted and a local server started. If you have an OpenAI key, you can use the OpenAI GPT model as follows:

```python
from llama_index.llms.openai import OpenAI
llm = OpenAI()
```
Alternatively, if you have another local model that can be started by Ollama, you can use:

```python
from llama_index.llms.ollama import Ollama
llm = Ollama(model="mistral", request_timeout=30.0)
```
2. Modify the code to use the desired embedding model:

```python
st.session_state.embed_model = resolve_embed_model("local:BAAI/bge-small-zh-v1.5")
```
For example, if you have an OpenAI key, you can use the OpenAI embedding model:

```python
from llama_index.embeddings.openai import OpenAIEmbedding
st.session_state.embed_model = OpenAIEmbedding()
```
Or you can use other Hugging Face embeddings:
```python
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
st.session_state.embed_modelSettings.embed_model = HuggingFaceEmbedding(
    model_name="BAAI/bge-small-en-v1.5"
)
```
3. Run the following command:
```shell
streamlit run MrWiki.py
```
After the program runs, it will create 'documents' and 'chroma_db' folders in the running directory.

User-uploaded original files are automatically saved in the 'documents' folder for reference (if the space usage is too large, deleting this folder will not affect the program). The embedded files are saved in the 'chroma_db' folder.

First, upload the files you want to consult in the sidebar. This step will persist the files into the vector database. Then, below the sidebar, select the files you want to query for this session. After confirming, click "Query these documents!" in the sidebar to load the database. Finally, ask your questions on the right.

For subsequent use, if you are consulting the same files, there is no need to re-upload. Simply select them below the sidebar.

If you do not want files to remain in the database, you can delete them. Next time you use the file, you will need to upload it again.

# Frequently Asked Questions
1. File upload size limit
In the project directory:
```shell
mkdir .streamlit && echo -e '[server]\nmaxUploadSize = 500' > .streamlit/config.toml
```
This will increase the limit to 500MB.