# Mr.Wiki

[en](README.md) | [简体中文](docs/README_zh.md)


<div align="center">
![Mr.Wiki](docs/sample.gif)
</div>

Mr.Wiki 是一个使用 Python3 开发的基于本地LLM 的使用向量数据库的 RAG 检索聊天机器人，采用了如下组件：
- [streamlit](https://streamlit.io/) 用于前端界面构建
- [llamaindex](https://www.llamaindex.ai/) LLM 数据框架
- [chromadb](https://www.trychroma.com/) 用于向量数据库

特点如下：
- 小而简单（仅有一个小于 160 行的 Python3 文件）
- 拥有一个用户友好的 Web 界面
- 界面上可以直接对向量数据库进行增加文档、删除文档、选择对应文档进行查询
- 向量数据库持久化在本地

# 安装方法
使用下列指令，从 Github 上下载仓库，创建 Python 虚拟环境并安装依赖。
```
git clone git@github.com:QSCTech-Sange/Mr.Wiki.git
cd Mr.Wiki && mkdir pyenv
cd pyenv && python3 -m venv . 
cd bin && source activate
cd ../.. && pip3 install -r requirements.txt
```
如果你确定不需要虚拟环境，可以直接
```
git clone git@github.com:QSCTech-Sange/Mr.Wiki.git
cd Mr.Wiki
pip3 install -r requirements.txt
```

# 使用方法
1. 修改代码中大语言模型为你所需要的
```python
llm = OpenAI(api_base="http://localhost:1234/v1", api_key="not-needed")
```
我这里的代码是使用了 [LM Studio](https://lmstudio.ai/) ，挂载了 
qwen1_5-14b-chat-q3_k_m.gguf 并启动了一个 local server。

假如你拥有 OpenAI 的 key，可以使用 OpenAI 的 GPT 模型。
```
from llama_index.llms.openai import OpenAI
llm = OpenAI()
```
或者，你拥有其他可以被 [Ollama](https://ollama.com/) 启动的本地模型，你也可以
```
from llama_index.llms.ollama import Ollama
llm = Ollama(model="mistral", request_timeout=30.0)
```


2. 修改代码中这一段 embedding 模型为你所需要的
```python
st.session_state.embed_model = resolve_embed_model("local:BAAI/bge-small-zh-v1.5")
```
例如，假如你拥有 OpenAI 的 key，你可以使用 OpenAI 的 embedding 模型。
```
from llama_index.embeddings.openai import OpenAIEmbedding
st.session_state.embed_model = OpenAIEmbedding()
```
或者，你可以使用其他 huggingface 上的 embedding 模型
```
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
st.session_state.embed_modelSettings.embed_model = HuggingFaceEmbedding(
    model_name="BAAI/bge-small-en-v1.5"
)
```
3. 接着运行
```
streamlit run MrWiki.py
```
程序运行后，会在运行目录下创建 documents 文件夹和 chroma_db 文件夹。

用户上传的原文件会自动保存在 documents 文件夹下（主要用途是便于查阅，如果占用空间过大的话，删除该文件夹不影响程序运行）。

embedding 后的文件则保存在 chroma_db 下。

首先侧边栏需要上传需要咨询的文件，该步骤会将文件持久化到向量数据库当中。其次在侧边栏下方选中本次需要启用查询的文件。确认后点击侧边栏 Query these documents! 之后会加载数据库。最后在右边提问即可。

之后使用的时候，如果咨询相同的文件，则不需要重新上传。直接在侧边栏下方选择即可。

如果不希望文件留在数据库当中，可以将它们删除。下一次使用该文件需要重新上传。


# 常见问题
1. 上传文件大小限制
