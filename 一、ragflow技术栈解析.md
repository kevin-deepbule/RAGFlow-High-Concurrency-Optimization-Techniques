# RAGFlow技术栈解析：从文档处理到智能问答的完整架构

## 技术架构概览
RAGFlow是一个面向知识问答的**端到端检索增强生成（RAG）系统**，其架构设计紧密围绕“问题理解-文档处理-智能召回-答案生成”的核心流程展开。下图展示了其模块化分层结构和数据流转逻辑：
![image](https://github.com/user-attachments/assets/103127d4-c650-4d58-83a7-46148efc14b0)

## 一、核心组件详解
### 1. 输入层 (Input Sources)
- **Questions**：用户实时提问入口，支持自然语言查询
- **Documents**：支持文件上传（PDF/Word等），通过Web Nginx访问存储
- **Task Dispatch**：接收文档处理任务，动态调度至任务执行器

### 2. 文档处理引擎 (Document Processing Engine)
| 模块                      | 功能说明                                                                 |
|--------------------------|--------------------------------------------------------------------------|
| Document Parser           | 文档格式解析器，支持多类型文件解析                                       |
| OCR                       | 处理图像/扫描文档的文本提取                                              |
| Document Layout Analyze   | 识别文档版式结构（标题/段落/图片区等）                                   |
| Table Structure Recognition | 专用表格解析器，保留行列语义结构                                          |
| → **Chunk**               | 输出标准化的文本块，保留元数据和位置信息                                  |

### 3. 查询处理层 (Query Processing)
- **Query Analyze**：问题语义解析与意图识别
- **Keyword & Embedding**：双路处理<br>
  • 关键词提取：传统检索特征<br>
  • 向量嵌入：深度语义表征（e.g., BERT类模型）

### 4. 混合召回层 (Hybrid Retrieval)
```mermaid
graph LR
A[Query分析结果] --> C[Multi-way Recall]
B[文档Chunk] --> C
C --> D[Re-rank]
