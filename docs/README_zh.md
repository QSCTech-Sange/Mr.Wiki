# Mr.Wiki

[en](README.md) | [简体中文](docs/README_zh.md)

Mr.Wiki 是一个使用 Python3 开发的基于本地LLM 的 RAG 检索聊天机器人，采用了如下组件：
- [streamlit](https://streamlit.io/) 用于前端界面构建
- [llamaindex](https://www.llamaindex.ai/) LLM 数据框架
- [chromadb](https://www.trychroma.com/) 用于向量数据库

特点如下：
- 小而简单（仅有一个小于 160 行的 Python3 文件）
- 拥有一个用户友好的 Web 界面
- 界面上可以直接对向量数据库进行增加文档、删除文档、选择对应文档进行查询
- 向量数据库持久化在本地

# 安装方法
1. 创建 Python 虚拟环境

2. 安装依赖

3. 启动 LLM 后端（可选）

# 使用方法
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

2. 自定义 LLM 

3. 自定义 embedding