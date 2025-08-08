# GPT-5 模型对接教程（基于急速API）

## 概述
GPT-5 是 OpenAI 迄今为止最智能的模型，尤其擅长：
- 代码生成、错误修复与重构
- 指令遵循
- 长上下文处理和工具调用

伴随模型发布的新 API 功能包括：控制冗长性、最小推理工作选项、自定义工具及允许的工具列表。本教程将介绍 GPT-5 系列的关键功能及基于急速 API 的对接方法（急速 API 已全面支持 GPT-5 系列模型）。


## 模型版本与分组支持
### 支持的模型分组
- `gpt-5-2025-08-07`：官转 openai / 优质官转 openai
- `gpt-5-chat-latest`：无思考模式
- `gpt-5-mini-2025-08-07`：限时特价 / default / 纯 az
- `gpt-5-nano-2025-08-07`：限时特价 / default / 纯 az

### 注意事项
- 最新模型不支持 `max_tokens` 参数
- 仅支持温度参数为 1


## 认识 GPT-5 系列模型
GPT-5 系列包含三种模型，各有侧重：

| 变体 | 最适合的场景 |
|------|------------|
| gpt-5 | 复杂推理、广阔世界知识、代码密集型或多步骤代理任务 |
| gpt-5-mini | 成本优化的推理和聊天，平衡速度、成本与功能 |
| gpt-5-nano | 高通量任务，尤其是简单指令遵循或分类 |


## GPT-5 新 API 功能
### 1. 最少推理工作（reasoning.effort）
控制模型生成响应前的推理标记数，可选值包括：
- `low`：优先速度，推理标记少
- `medium`：默认值，平衡速度与推理质量
- `high`：更彻底的推理，适合复杂任务
- `minimal`：生成极少推理标记，首个标记响应最快，适合编码和指令遵循场景

### 2. 冗长性控制
调节输出令牌数量，影响延迟和回答详尽程度：
- **高冗长性**：适合详尽解释文档、广泛代码重构
- **低冗长性**：适合简洁答案或简单代码生成（如 SQL 查询）


## 对接指南（基于急速 API）
### 1. 准备工作
- 获取急速 API 密钥（访问 [急速 API 官网](https://api.jisuai.top/) 获取）
- 确保网络可访问急速 API 端点：`https://api.jisuai.top`


### 2. 两种对接方式
#### 方式一：Responses API（支持思维链传递）
适用于需要模型展示思考过程的场景，需使用 `responses` 格式（参考：[详细文档](https://jusu-api.apifox.cn/333591658e0)）。

**请求示例（curl）**：
```bash
curl --request POST \
--url https://api.jisuai.top/v1/responses \
--header "Authorization: Bearer $OPENAI_API_KEY" \
--header 'Content-type: application/json' \
--data '{
  "model": "gpt-5",
  "input": "How much gold would it take to coat the Statue of Liberty in a 1mm layer?",
  "reasoning": {
    "effort": "minimal"
  }
}'
```

#### 方式二：Chat Completions API（无思维链传递）
适用于直接获取答案的场景。

**请求示例（curl）**：
```bash
curl --request POST \
--url https://api.jisuai.top/v1/chat/completions \
--header "Authorization: Bearer $OPENAI_API_KEY" \
--header 'Content-type: application/json' \
--data '{
  "model": "gpt-5",
  "messages": [
    {
      "role": "user",
      "content": "How much gold would it take to coat the Statue of Liberty in a 1mm layer?"
    }
  ],
  "reasoning_effort": "minimal"
}'
```


## 迁移指南：从 Chat Completions 到 Responses API
主要区别在于 Responses API 支持回合间传递思维链（CoT），可提升智能表现、减少推理令牌、提高缓存命中率并降低延迟。两者参数格式略有不同，核心功能保持一致。


## 更多资源
- 详细对接教程：[https://jusu-api.apifox.cn/333591658e0](https://jusu-api.apifox.cn/333591658e0)
- 急速 API 官网：[https://api.jisuai.top/](https://api.jisuai.top/)
