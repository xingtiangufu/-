# 🦋 星光咖啡馆 AI 助手

> 一个自带 Web UI 的本地 AI 助手，集成文件操作、系统命令、联网搜索、微信控制、长期记忆、语音识别、安全防护等功能。
>
> 从 GitHub clone 下来，填个 API Key，双击启动，就能用。

![Python 3.9+](https://img.shields.io/badge/Python-3.9+-blue?logo=python)
![License](https://img.shields.io/badge/License-MIT-green)

---

## ✨ 功能一览

| 功能 | 说明 |
|------|------|
| 💬 **AI 对话** | 支持流式输出、工具调用、图片识别 |
| 📁 **文件操作** | 读取、写入、创建、列出文件和目录 |
| 💻 **系统命令** | 在本地执行 shell/cmd 命令 |
| 🌐 **联网搜索** | 实时联网搜索信息（DuckDuckGo） |
| 💬 **微信控制** | 搜索联系人、填入消息、发送消息（Windows） |
| 🧠 **长期记忆** | BM25 + TF-IDF 混合检索，自动上下文注入，对话压缩 |
| 🎤 **语音识别** | 麦克风录音 + Whisper 离线识别 |
| 📤 **文件上传** | 上传文件供 AI 助手读取处理 |
| 🖼️ **图片识别** | 拍照/选图发送给 AI 分析 |
| 🛡️ **安全防护** | 模糊匹配引擎 + 速率限制 + 内容过滤 + 审计日志 |
| 🔊 **语音播报** | 浏览器端 TTS 语音播报回复 |

---

## 🚀 快速开始（3 步搞定）

### 1️⃣ 克隆项目

```bash
git clone https://github.com/你的用户名/你的仓库名.git
cd 你的仓库名
```

### 2️⃣ 填入 API Key

打开 `.env` 文件，将 `API_KEY` 改成你自己的：

```env
API_KEY=你的真实API_Key
BASE_URL=https://open.bigmodel.cn/api/paas/v4
MODEL_NAME=glm-4-flash
```

> **API Key 获取方式：**
> - 智谱AI（推荐新手）：[https://open.bigmodel.cn](https://open.bigmodel.cn) → 注册 → 创建 API Key（免费额度）
> - OpenAI：[https://platform.openai.com](https://platform.openai.com) → API Keys
> - 其他兼容 OpenAI 格式的模型服务也可以，修改 `BASE_URL` 即可

### 3️⃣ 启动

**Windows：** 双击 `start.bat`

**Mac / Linux：**
```bash
chmod +x start.sh
./start.sh
```

**手动启动：**
```bash
pip install -r requirements.txt
python app.py
```

启动成功后自动打开浏览器 → `http://localhost:5000` 🎉

---

## 📁 项目结构

```
.
├── app.py                  # 主程序（Flask 服务器）
├── client_mcp.py           # AI 对话核心（工具调用 + 流式输出）
├── security_server.py      # 安全中间层（模糊匹配 + 速率限制）
├── config.json             # 全局配置（UI / 工具 / 安全 / TTS）
├── requirements.txt        # Python 依赖
├── .env.example            # 环境变量模板
├── .env                    # 你的实际配置（不入库）
├── start.bat               # Windows 一键启动
├── start.sh                # Mac/Linux 一键启动
├── README.md               # 你正在看的文件
│
├── static/
│   ├── index.html          # 聊天主界面
│   └── upload.html         # 文件上传界面
│
├── tools/                  # AI 工具插件（模块化，可按需开关）
│   ├── __init__.py         # 工具加载器
│   ├── file_tools.py       # 📁 文件读写
│   ├── system_tools.py     # 💻 命令执行
│   ├── web_tools.py        # 🌐 联网搜索
│   ├── wechat_tools.py     # 💬 微信控制
│   └── memory_tools.py     # 🧠 长期记忆
│
├── prompts/
│   └── system_prompt.md    # AI 角色提示词
│
├── chat_data/              # 运行时数据（自动创建）
│   ├── memory.jsonl        # 长期记忆持久化
│   └── memory.db           # FTS5 全文索引
│
└── uploads/                # 用户上传文件（自动创建）
```

---

## ⚙️ 配置说明

所有配置都在 `config.json` 中，修改后**重启生效**。

### 自定义 AI 人设

编辑 `prompts/system_prompt.md`，改成你想要的任何角色和风格。

### 开关工具

在 `config.json` 的 `tools.enabled` 中，`true`/`false` 控制各工具的启用：

```json
{
  "tools": {
    "enabled": {
      "file_tools": true,
      "system_tools": true,
      "web_tools": true,
      "wechat_tools": true,
      "memory_tools": true
    }
  }
}
```

### 安全配置

- `safety.payment_keywords` — 敏感词列表（中文）
- `safety.payment_keywords_en` — 敏感词列表（英文）
- `safety.payment_pinyin` — 敏感词拼音列表（防拼音绕过）
- `security_server.rate_limit` — 速率限制
- `security_server.content_filter` — 内容过滤
- `security_server.audit_log` — 审计日志

### 切换 AI 模型

修改 `.env`：

```env
# 智谱 AI（推荐，免费额度）
API_KEY=你的Key
BASE_URL=https://open.bigmodel.cn/api/paas/v4
MODEL_NAME=glm-4-flash

# OpenAI
API_KEY=sk-xxx
BASE_URL=https://api.openai.com/v1
MODEL_NAME=gpt-4o-mini

# DeepSeek
API_KEY=你的Key
BASE_URL=https://api.deepseek.com/v1
MODEL_NAME=deepseek-chat

# Ollama 本地模型（完全离线）
API_KEY=ollama
BASE_URL=http://localhost:11434/v1
MODEL_NAME=qwen2.5:7b
```

---

## 🛡️ 安全机制

项目内置多层安全防护（`security_server.py`）：

| 层级 | 说明 |
|------|------|
| **原文匹配** | 精确检测中文/英文敏感关键词 |
| **标准化匹配** | 全角→半角、Unicode 规范化、去分隔符、合并重复字符 |
| **拼音匹配** | 检测拼音输入的敏感词绕过 |
| **代字检测** | 检测数字/字母替代变体（如"转Z账"） |
| **速率限制** | 滑动窗口限流，防止滥用 |
| **内容过滤** | 输入长度限制 + 自定义正则拦截 |
| **审计日志** | 所有请求记录到 `security_audit.log` |

---

## 🧠 记忆系统

内置基于 **BM25 全文检索 + TF-IDF 语义相似度 + 时间衰减** 的混合记忆系统：

- **自动注入**：每次对话前自动检索相关记忆注入上下文
- **对话压缩**：对话历史过长时自动将旧内容压缩写入记忆
- **记忆管理**：通过 Web UI 的「🧠 记忆库」按钮可视化管理
- **混合检索**：FTS5 全文索引 + TF-IDF 向量 + 时间衰减 + 重要度加权融合

---

## 📋 可选依赖

| 功能 | 额外安装 |
|------|----------|
| 微信控制 | Windows + 微信桌面版 + `pip install pyautogui pygetwindow pyperclip pytesseract` + [Tesseract OCR](https://github.com/UB-Mannheim/tesseract/wiki) |
| 语音识别 | 麦克风 + `pip install SpeechRecognition pyaudio openai-whisper` |
| 本地模型 | [Ollama](https://ollama.ai) |

---

## ❓ 常见问题

<details>
<summary><b>启动报错 No module named 'xxx'</b></summary>

依赖没装全，运行：
```bash
pip install -r requirements.txt
```
</details>

<details>
<summary><b>API Key 怎么获取？</b></summary>

推荐使用[智谱AI](https://open.bigmodel.cn)（国产、免费额度、无需翻墙），注册后在控制台创建 API Key 即可。也支持 OpenAI、DeepSeek、Ollama 等任何兼容 OpenAI 格式的 API。
</details>

<details>
<summary><b>支持哪些系统？</b></summary>

- Windows 10/11 ✅（全功能）
- macOS ✅（除微信控制外全功能）
- Linux ✅（除微信控制外全功能）
</details>

<details>
<summary><b>可以完全离线运行吗？</b></summary>

可以！安装 [Ollama](https://ollama.ai) 后配置：
```env
API_KEY=ollama
BASE_URL=http://localhost:11434/v1
MODEL_NAME=qwen2.5:7b
```
联网搜索功能需要网络，其他功能均可离线使用。
</details>

<details>
<summary><b>如何修改 AI 的角色/人设？</b></summary>

编辑 `prompts/system_prompt.md` 文件，写上你想要的任何角色设定和规则。
</details>

---

## 📝 许可证

MIT License — 随便用，随便改，开心就好 ☕🦋
