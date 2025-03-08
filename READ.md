# 什么是 LangChain?

LangChain 是一个用于开发由语言模型驱动的应用程序的框架

# LangChain 的设计思路？

- 模块化

- 抽象化

把和大型语言模型交互的流程抽象出来，变成一个一个的小模块，方便更好的进行调用。通过把这些小模块串联起来，实现更多更复杂的功能

# LangChain 解决的问题

- 如何格式化输出？
- 如何输入很长的文本？
- 如何多次进行 API 的调用？
- 如何让 API 能够调用外部的服务？
- 如何进行标准化的开发？

# 创建第一个 LLM 模块

- 本地运行一个开源的 LLM 模型
  - 需要庞大的 GPU 资源
- 使用第三方的模型 API
  - 百度 文心 ERNIE
  - 阿里 通义千问
- 简单应用-用 LLM 模块生成文本
  - 在 langchain 中，一个 LLM 模块的最基本的功能就是根据输入的文本生成新的文本 ，方法名称:predict()

* 生成的文本结果会根据底层模型的不同有很大的差别，而且 LLM 模块所设置的 tempurate 参数也会影响生成的文本的质量。

# 简单应用-用聊天模块续写对话

- 聊天模块（chat model）
  - 和 LLM 模块类似，以 LLM 为底层支持
- 用于支持聊天模块的特殊的类
  - AIMessage
  - HumanMessage
  - SystemMessage
- 聊天模块支持的方法的调用
  - predict()
  - predict_messages()
    - 使用方法和 LLM 模块一样

# 简单应用-链式结构

- 为什么要链式结构？
  - 方便的连接多个（llm）模块
- 如何避免重复定义功能相似的 llm 模块？
  - 提示模板

# 简单应用-代理人

- 为什么要代理人
  - 处理链式模式难以独立处理的复杂问题
  - 动态决策
- 本质
  - 对于一个任务，运用语言模型来决定完成任务所需的行为以及实施这些行为的顺序
- 行为方式
  - 代理人可以使用一系列预设的工具
    - 选择工具
    - 使用工具
    - 观察并处理工具使用的结果
    - 重复以上步骤

# 与语言模型交互的"编程方式"——提示（prompt）

- 精准的提示 =>理想的结果
- 冗余的提示 => 稳定的结果

# LangChain 对提示的封装 - 提示模板（PromptTemplates）

- 提示模板可以包括: -对语言模型的指令
  - 提供一些简单示例给语言模型从而使模型输出更接近理想结果
  - 提示语言模型的问题
- 生成一个提示模板
  - 无输入参数
  - 单个输入参数
  - 多个输入参数
- 对话提示模板
- 接受部分参数的提示模板
  - 使用场景：所有的参数无法同步获得。当在链式结果中传递提示模板时，不需要一次性提供所以的参数
- 接受部分参数的提示模板
  - 使用场景：所有的参数无法同步获得。当在链式结果中传递提示模板时，不需要一次性提供所以的参数

# 提供示例给语言模型从而实现逻辑思维

- 少样本学习（few-shot）
  - 通过少量的样本（案例）让语言模型学会处理某类特定问题
  - 和对语言模型本身的微调（fine-tuning）区分
- 利用样本选择器筛选样本
  - 为什么需要样本选择器（ExampleSelector）
    - 样本数量太多
    - 不是所有样本都能起到提升输出质量的作用

# 大型语言模型的封装 —— llm 模块

- LangChain 不提供现成的大型语言模型
- LangChain 只提供针对不同语言模型的标准化接口

# llm 模块基本用法

- 直接调用
  - 功能上类似于调用 predict(),让 llm 模块根据输入续写文本
  - 批量生成
    - generate()
    - 输入:文本的列表
    - 输出:文本的列表

# 自定义 llm 模块

- 用于封装 LangChain 尚未支持的大型语言模型
- 也可以用来模拟测试
- 你来定义当该 llm 模块的时候，如何根据输入的文本来输出

# 数据连接

- 一些基于大型语言模型的应用经常需要用到模型数据集中没有的数据。针对这一需求，LangChain 提供了一系列的工具可以让你从各种数据源中加载新的数据，转换数据，存储数据以及访问数据
  - 文档加载器：从多种不同的数据源加载文档
  - 文档转换器： 拆分文档，丢弃冗余文档等
  - 文本 embedding 模型：将非结构化文本转换为浮点数的列表
  - 向量存储站：存储和搜索 embedding 数据
  - 检索器：查询向量数据
- 数据连接处理流程
  - Source->Load->Transform->Embed->Store->Retrieve

# 数据加载器（Document loaders）

- 输入：各种数据源
  - PDF
    -URL
    -Excel
- 输出：一系列的 Document 对象
  PDF->PDF Loader->Documents

# 文档转换器（Document transformers）

- 文档转换器处理流程
  - Source->Load->Transform->Embed->Store->Retrieve

# 文本嵌入模型

- 文本嵌入模型处理流程
  - Source->Load->Transform->Embed->Store->Retrieve

# 向量存储站

- 向量存储站处理流程
  - Source->Load->Transform->Embed->Store->Retrieve

# 检索器

- 检索器处理流程
  - Source->Load->Transform->Embed->Store->Retrieve

# 什么是链结构？

- LangChain 的模块化，抽象化能力
  定义提示词->选择 LLM->运行 inferencing->得到结果
- 概念上，链是在规定的标准下，不同模块的组合
- 链是一个抽象出来的概念/标准，并不是一定要用链才能完成对 LLM 的调用
- 链提高了模块的标准化，复用性
- 链增加了工程的复杂度，冗余度

# 链结构分类

- 基础链结构
  - LLM chain
  - Router chain
  - Sequential chain
  - Transformation chain
- 应用链结构实例
  - Document chains
    -Retrieval QA

# LLM chain(单链结构)

1. 定义 prompt
2. 定义 llm
3. 定义 chain
4. 运行 predict

# Router chain(多链结构)

1. 定义 prompts
2. 定义 llms/embeddings
3. 定义 chain（多链并行，选择一条链处理）
4. 运行 predict

# Sequential chain(多链结构)

1. 定义 prompts
2. 定义 llms/embeddings
3. 定义 chain（多链串行，按顺序处理）
4. 运行 predict

# Transformation chain(多链结构)

# Document chains type(长文本处理链)

- Stuff
- Refine
- Map Reduce
- Map Rerank

# Retrieval QA
