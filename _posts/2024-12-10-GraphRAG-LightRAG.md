---
layout:     post
title:      "GraphRAG和LightRAG"
date:       2024-12-09
author:     "Jack Ding"
header-style: text
tags:
    - LLM
    - tutorial
---

[LLM论文研读: GraphRAG的替代者LightRAG - 简书 (jianshu.com)](https://www.jianshu.com/p/fd3a3a3a7c00)

1.背景
最近有一个很火的开源项目LightRAG，Github6.4K+星※，北邮和港大联合出品，是一款微软GraphRAG的优秀替代者，因此本qiang~得了空闲，读读论文、跑跑源码，遂有了这篇文章。

2. LightRAG框架
2.1已有RAG系统的局限性
1) 许多系统仅依赖于平面数据表示(如纯文本)，限制了根据文本中实体间复杂的关系来理解和检索信息的能力。

2) 许多系统缺乏各种实体及其关系之间保持一致所需的上下文意识，导致可能无法完全解决用户的问题。

2.2 LightRAG的优势
1) 引入图结构：将图结构引入文本索引及相关信息检索的环节中，图结构可以有效表示实体及其关系，有利于上下文的连贯性与丰富性。

2) 综合信息检索: 从所有文档中提取相互依赖的实体的完整上下文，以确保信息检索的综合性。相对于传统的RAG，可能只关注于Chunk后的局部文本，缺乏全局综合信息。

3) 增强检索效率: 提高基于图结构的知识检索效率，以显著减少响应时间。

4) 新数据的快速适配: 能够快速适应新的数据更新，确保系统在动态环境中保持相关性。

5) 减少检索开销: 相对于GraphRAG以社区遍历的方法，LightRAG专注于实体和关系的检索，进而减少开销。

2.3 LightRAG的框架



LightRAG将基于图结构的文本索引(graph-based text indexing)无缝地集成到一个双层检索框架(dual-level retrieval framework)中，因此能够提取实体间复杂的内部关系，提高响应的丰富性和连贯性。

双层检索策略包括低级检索和高级检索，其中低级检索重点关注特定实体及其关系的准确信息，高级检索则包含了广泛的主题信息。

此外，通过将图结构与向量表征相结合，LightRAG促进了相关实体和关系的有效检索，同时基于结构化的知识图谱中相关的信息，增强了结果的全面性。

LightRAG无需重复构建整个索引，降低了计算成本且加速了适配，而且其增量更新算法保障了新数据的及时整合。

2.3.1 基于图的文本索引
1) 实体及关系抽取：LightRAG先将大文本切分为小文本，然后利用LLM识别并抽取小文本中各种实体及其关系，此举可便于创建综合的知识图谱，prompt示例如下：


2) 使用LLM性能分析功能生成键值对：使用LLM提供的性能分析函数，为每个实体及每条关系生成一个文本键值对(K, V)，其中K是一个单词或短语，便于高效检索，V是一个文本段落，用于文本片段的总结

3) 去重以优化图操作：通过去重函数识别并合并来自不同段落的相同实体和关系。有效地减少了与图操作相关的开销，通过最小化图的大小，从而实现更高效的数据处理。

2.3.2 双层检索机制
1) 在细节层和抽象层分别生成查询键：具体查询以细节为导向，许精确检索特点节点或边相关信息；抽象查询更加概念化，涵盖更广泛的主题、摘要，其并非与特定实体关联。

2) 双层检索机制：低级检索聚焦于检索特定实体及其属性或关系信息，旨在检索图谱中指定节点或边的精确信息；高级检索处理更广泛的主题，聚合多个相关实体和关系的信息，为高级的概念及摘要提供洞察力。

3) 集成图以及向量以便高效检索：通过图结构和向量表示，使得检索算法有效地利用局部和全局关键词，简化搜索过程并提高结果的关联性。具体分为如下步骤：

a. 查询关键词提取：针对给定的问题，LightRAG的检索算法首先分别提取局部查询关键词和全部查询关键词

关键词提取的prompt如下：


b. 关键词匹配：检索算法使用向量数据库来匹配局部查询关键词与候选实体，以及全局查询关键词与候选关系(与全局关键词关联)

c. 增强高阶关联性: LightRAG进一步收集已检索到的实体或关系的局部子图，如实体或关系的一跳邻近节点

2.3.3 检索增强回答生成
1) 使用已检索信息: 利用已检索的信息，包括实体名、实体描述、关系描述以及原文片段，LightRAG使用通用的LLM来生成回答。

2) 上下文集成及回答生成: 将查询串与上下文进行整合，调用LLM生成答案。

2.3.4 整体过程示例

3. 实验
3.1 数据源
从UltraDomain基准中选取了4个数据集，分别包括农业、计算机科学、法律、混合集，每个数据集包含60W-500W个token。


3.2 问题生成
为了评估LightRAG的性能，首先通过LLM生成5个RAG用户，且为每个用户生成5个任务。每个用户均具有描述信息，详细说明了他们的专业知识和特征，以引发他们提出相关问题。每个用户任务也具有描述信息，强调其中一个用户在于RAG交互时的潜在意图。针对每个用户任务的组合，LLM生成5个需要理解整个数据集的问题。因此，每个数据集共产生125个问题。

问题生成的prompt如下：


3.3 基线模型
选取的4个基线模型包括Naive RAG, RQ-RAG, HyDE, GraphRAG。

3.4评价维度及细节
实验中，向量检索采用nano 向量库，LLM选择GPT-4o-mini，每个数据集的分块大小为1200，此外收集参数(gleaning parameter，目的在于仅通过1轮LLM无法完全提取对应的实体或关系，因此该参数旨在增加多次调用LLM)设置为1。

评价标准采用基于LLM的多维度比较方法，使用GPT-4o-mini针对LightRAG与每个基线的响应进行排名。主要包含如下4个维度:全面性(回答多大程度解决了问题的所有方面和细节)、多样性(与问题相关的不同观点，答案的多样性和丰富性如何)、接受度(答案是否有效使读者理解主题并做出明确判断)、整体评价(评估前三个标准的累积评价)。

评价prompt如下：


3.5 实验结果
3.5.1 与基线RAG方法比较

3.5.2 双层检索及基于图结构的索引增强消融结果

3.5.3 具体示例研究

3.5.4 与GraphRAG的成本比较

4. 整体工作流
图片建议放大，看的更清楚~

LightGraph的源码可读性非常强，建议看官们可以基于上面这张流程图，逐步调试LightGraph，以了解其检索和生成两个模块的具体细节。



要使用 Vue 3 创建一个大模型对话网页，你可以参考以下步骤来实现前端框架和后端集成。以下是基本的步骤，涵盖了 Vue 3 的实现、API 调用以及显示聊天界面。

### 1. 创建 Vue 3 项目

首先，创建一个新的 Vue 3 项目。如果还没有安装 Vue CLI，可以通过以下命令安装：

```
bash


复制代码
npm install -g @vue/cli
```

然后，创建一个新项目：

```
bash复制代码vue create large-model-chat
cd large-model-chat
```

选择默认配置或者手动选择配置。

### 2. 安装依赖

你需要安装一些必备的依赖，比如 Axios（用于向后端发送 API 请求）。

```
bash


复制代码
npm install axios
```

### 3. 创建聊天组件

在 `src/components/` 目录下创建一个 `ChatComponent.vue` 文件，构建聊天界面。

```
vue复制代码<template>
  <div class="chat-container">
    <div class="chat-box">
      <div v-for="message in messages" :key="message.id" class="message">
        <div :class="message.type">{{ message.text }}</div>
      </div>
    </div>
    <div class="input-box">
      <input v-model="userMessage" @keyup.enter="sendMessage" placeholder="Type your message..." />
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      userMessage: "",
      messages: [],
    };
  },
  methods: {
    async sendMessage() {
      if (this.userMessage.trim()) {
        // Display user message
        this.messages.push({
          id: Date.now(),
          text: this.userMessage,
          type: "user",
        });

        // Send message to backend (large model)
        try {
          const response = await axios.post("http://your-backend-url/api/chat", {
            message: this.userMessage,
          });

          // Display model response
          this.messages.push({
            id: Date.now() + 1,
            text: response.data.reply,
            type: "model",
          });
        } catch (error) {
          console.error("Error sending message:", error);
          this.messages.push({
            id: Date.now() + 1,
            text: "Sorry, there was an error processing your request.",
            type: "model",
          });
        }

        // Clear input field
        this.userMessage = "";
      }
    },
  },
};
</script>

<style scoped>
.chat-container {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 16px;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.chat-box {
  height: 400px;
  overflow-y: auto;
  margin-bottom: 16px;
}

.message {
  padding: 8px;
  margin-bottom: 8px;
  border-radius: 4px;
  max-width: 80%;
}

.user {
  background-color: #d1f7d1;
  align-self: flex-start;
}

.model {
  background-color: #f1f1f1;
  align-self: flex-end;
}

.input-box {
  display: flex;
  gap: 8px;
}

input {
  flex-grow: 1;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

### 4. 集成后端 API

假设你已经有一个后端服务，它接收用户消息并返回大模型的响应。如果没有，你可以设置一个简单的后端，使用 Flask 或 Node.js 来处理 API 请求。

例如，后端 API 示例（Node.js）：

```
javascript复制代码// server.js (Node.js example with Express)
const express = require("express");
const axios = require("axios");
const app = express();
const port = 3000;

app.use(express.json());

app.post("/api/chat", async (req, res) => {
  const userMessage = req.body.message;

  try {
    const response = await axios.post("YOUR_LARGE_MODEL_API_URL", {
      message: userMessage,
    });

    res.json({ reply: response.data.reply });
  } catch (error) {
    res.status(500).json({ reply: "Error processing your request." });
  }
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

### 5. 连接前后端

- 在 Vue 组件中的 `axios.post()` 方法中，替换为你的后端 API URL。例如：`http://localhost:3000/api/chat`。
- 后端会接收到用户消息，调用大模型 API，然后返回模型的回答。

### 6. 启动应用

启动 Vue 应用和后端服务器。

1. 启动 Vue 应用：

   ```
   bash
   
   
   复制代码
   npm run serve
   ```

2. 启动 Node.js 后端（如果使用 Node.js）：

   ```
   bash
   
   
   复制代码
   node server.js
   ```

现在，你应该可以在浏览器中访问 Vue 前端，输入消息并得到大模型的响应。

### 7. 后续改进

- **样式优化**：根据需要调整样式，增加聊天气泡、滚动条等。
- **身份认证**：如果需要身份验证，可以在前端和后端实现认证逻辑。
- **大模型的调用优化**：如果调用的是一个大型语言模型，考虑如何处理超时和并发请求。

如果有任何问题或需要进一步的帮助，随时告诉我！