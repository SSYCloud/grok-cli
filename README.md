# Grok CLI

一个由 Grok 驱动的会话式 AI CLI 工具，具有智能文本编辑器功能和工具使用能力 。

<img width="980" height="435" alt="Screenshot 2025-07-21 at 13 35 41" src="https://github.com/user-attachments/assets/192402e3-30a8-47df-9fc8-a084c5696e78  " />

## 特性

- **🤖 会话式 AI**：由 Grok-3/Grok-4 提供支持的自然语言界面 
- **📝 智能文件操作**：AI 会自动使用工具查看、创建和编辑文件 
- **⚡ Bash 集成**：通过自然对话执行 shell 命令 
- **🔧 自动工具选择**：AI 会智能地为您的请求选择合适的工具 
- **💬 交互式 UI**：使用 Ink 构建的美观终端界面
- **🌍 全局安装**：通过 `npm i -g @vibe-kit/grok-cli` 在任何地方安装和使用

## 安装

### 先决条件
- Node.js 16+
- 来自 X.AI 的 Grok API 密钥

### 全局安装 (推荐)
```bash
npm install -g @vibe-kit/grok-cli
```

### 本地开发
```bash
git clone <repository>
cd grok-cli
npm install
npm run build
npm link
```

## 设置

1. 从 [胜算云](https://console.shengsuanyun.com/user/keys) 获取您的 Grok API 密钥

2. 设置您的 API 密钥 (选择一种方法):

**方法 1: 环境变量**
```bash
export GROK_API_KEY=your_api_key_here
```

**方法 2: .env 文件**
```bash
cp .env.example .env
# 编辑 .env 并添加您的 API 密钥
```

**方法 3: 命令行标志**
```bash
grok --api-key your_api_key_here
```

**方法 4: 用户设置文件**
创建 `~/.grok/user-settings.json`:
```json
{
  "apiKey": "your_api_key_here"
}
```

### 自定义基础 URL (可选)

您可以配置自定义的 Grok API 端点 (选择一种方法):

**方法 1: 环境变量**
```bash
export GROK_BASE_URL=https://your-custom-endpoint.com/v1  
```

**方法 2: 命令行标志**
```bash
grok --api-key your_api_key_here --baseurl https://your-custom-endpoint.com/v1  
```

**方法 3: 用户设置文件**
添加到 `~/.grok/user-settings.json`:
```json
{
  "apiKey": "你的胜算云API_KEY",
  "baseURL": "https://router.shengsuanyun.com/api/v1",
  "model":"x-ai/grok-4"
}
```

## 使用方法

### 交互模式

启动会话式 AI 助手:
```bash
grok
```

或者指定工作目录:
```bash
grok -d /path/to/project
```

### 无头模式 (Headless Mode)

处理单个提示并退出 (适用于脚本和自动化):
```bash
grok --prompt "show me the package.json file"
grok -p "create a new file called example.js with a hello world function"
grok --prompt "run npm test and show me the results" --directory /path/to/project
```

此模式特别适用于:
- **CI/CD 流水线**: 自动化代码分析和文件操作
- **脚本**: 将 AI 辅助功能集成到 shell 脚本中
- **终端基准测试**: 非常适合需要非交互式执行的 Terminal Bench 等工具
- **批处理**: 以编程方式处理多个提示

### 模型选择

您可以使用 `--model` 参数指定要使用的 AI 模型:

```bash
# 使用 Grok 模型
grok --model x-ai/grok-4
grok --model x-ai/grok-3

# 使用其他模型 (使用相应的 API 端点)
grok --model google/gemini-2.5-pro --base-url https://api-endpoint.com/v1  
grok --model anthropic/claude-sonnet-4 --base-url https://api-endpoint.com/v1  
```

### 命令行选项

```bash
grok [options]

Options:
  -V, --version          output the version number
  -d, --directory <dir>  set working directory
  -k, --api-key <key>    Grok API key (or set GROK_API_KEY env var)
  -u, --base-url <url>   Grok API base URL (or set GROK_BASE_URL env var)
  -m, --model <model>    AI model to use (e.g., grok-4-latest, grok-3-latest)
  -p, --prompt <prompt>  process a single prompt and exit (headless mode)
  -h, --help             display help for command
```

### 自定义系统提示词

您可以通过在项目目录中创建 `.grok/GROK.md` 文件来提供自定义系统提示词，以调整 Grok 的行为:

```bash
mkdir .grok
```

创建 `.grok/GROK.md` 并写入您的自定义系统提示词:
```markdown
# Grok CLI 的自定义系统提示词

Always use TypeScript for any new code files.
When creating React components, use functional components with hooks.
Prefer const assertions and explicit typing over inference where it improves clarity.
Always add JSDoc comments for public functions and interfaces.
Follow the existing code style and patterns in this project.
```

Grok 会在您项目目录中工作时自动加载并遵循这些系统提示词。自定义系统提示词会被添加到 Grok 的系统提示中，并优先于默认行为。

## 示例对话

无需输入命令，只需告诉 Grok 您想做什么:

```
💬 "Show me the contents of package.json"
💬 "Create a new file called hello.js with a simple console.log"
💬 "Find all TypeScript files in the src directory"
💬 "Replace 'oldFunction' with 'newFunction' in all JS files"
💬 "Run the tests and show me the results"
💬 "What's the current directory structure?"
```

## 开发

```bash
# 安装依赖
npm install

# 开发模式
npm run dev

# 构建项目
npm run build

# 运行 linter
npm run lint

# 类型检查
npm run typecheck
```

## 架构

- **Agent**: 核心命令处理和执行逻辑
- **Tools**: 文本编辑器和 bash 工具的实现
- **UI**: 基于 Ink 的终端界面组件
- **Types**: 整个系统的 TypeScript 定义

## 许可证

MIT