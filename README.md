# Weather MCP (Model Context Protocol) 天气查询服务

## 🌤️ 服务介绍

Weather MCP 是一个基于 Model Context Protocol (MCP) 的天气查询服务，通过集成 OpenWeather API 提供全球天气信息查询功能。该服务支持自然语言交互，用户可以用中文描述需求，系统会自动调用相应的天气查询工具并返回格式化的天气信息。

## 📋 服务描述

### 主要功能
- **全球天气查询**: 支持查询全球任意城市的实时天气信息
- **多语言支持**: 支持中文查询和中文天气描述
- **智能交互**: 通过大语言模型理解用户意图，自动调用天气查询工具
- **格式化输出**: 提供美观易读的天气信息展示

### 技术特点
- 基于 MCP 协议，支持与各种 AI 客户端集成
- 异步 HTTP 请求，提高响应速度
- 完善的错误处理和容错机制
- 支持 stdio 通信方式

## 🏗️ 类型

### 服务类型
- **MCP Server**: 提供天气查询工具的 MCP 服务器
- **MCP Client**: 支持与大语言模型交互的客户端
- **API Integration**: 集成 OpenWeather API 的天气数据服务

### 架构类型
- **微服务架构**: 服务器和客户端分离，支持独立部署
- **异步架构**: 基于 asyncio 的异步处理
- **协议驱动**: 基于 MCP 协议的标准化通信

## ⚙️ 服务配置

### 服务器配置 (server.py)

```python
# MCP 服务器配置
mcp = FastMCP("WeatherServer")

# OpenWeather API 配置
OPENWEATHER_API_BASE = "https://api.openweathermap.org/data/2.5/weather"
API_KEY = "your_api_key_here"
USER_AGENT = "weather-app/1.0"

# 请求参数配置
DEFAULT_PARAMS = {
    "units": "metric",  # 使用摄氏度
    "lang": "zh_cn"     # 中文语言
}
```

### 客户端配置 (client.py)

```python
class MCPClient:
    def __init__(self):
        self.openai_api_key = os.getenv("OPENAI_API_KEY")
        self.base_url = os.getenv("BASE_URL")
        self.model = os.getenv("MODEL")
        self.client = OpenAI(api_key=self.openai_api_key, base_url=self.base_url)
```

### 项目依赖配置

#### Python 依赖 (pyproject.toml)
```toml
[project]
name = "weather-mcp"
version = "0.1.0"
description = "Weather MCP Server for global weather queries"
dependencies = [
    "mcp>=1.0.0",
    "httpx>=0.24.0",
    "openai>=1.0.0",
    "python-dotenv>=1.0.0"
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=23.0.0",
    "flake8>=6.0.0"
]
```

#### Node.js 依赖 (package.json)
```json
{
  "name": "weather-mcp",
  "version": "0.1.0",
  "description": "Weather MCP Server",
  "dependencies": {
    "commander": "^11.0.0"
  }
}
```

## 🔧 环境变量配置

### 必需环境变量

创建 `.env` 文件并配置以下环境变量：

```bash
# OpenAI API 配置
OPENAI_API_KEY=your_openai_api_key_here
BASE_URL=https://api.openai.com/v1
MODEL=gpt-3.5-turbo

# OpenWeather API 配置 (在 server.py 中配置)
# API_KEY=your_openweather_api_key_here
```

### 环境变量说明

| 变量名 | 描述 | 示例值 | 是否必需 |
|--------|------|--------|----------|
| `OPENAI_API_KEY` | OpenAI API 密钥 | `sk-...` | ✅ 必需 |
| `BASE_URL` | OpenAI API 基础 URL | `https://api.openai.com/v1` | ✅ 必需 |
| `MODEL` | 使用的 OpenAI 模型 | `gpt-3.5-turbo` | ✅ 必需 |

### 获取 API 密钥

#### OpenAI API Key
1. 访问 [OpenAI Platform](https://platform.openai.com/)
2. 登录或注册账号
3. 进入 API Keys 页面
4. 创建新的 API Key
5. 复制密钥到 `.env` 文件

#### OpenWeather API Key
1. 访问 [OpenWeather API](https://openweathermap.org/api)
2. 注册免费账号
3. 获取 API Key
4. 在 `server.py` 中替换 `API_KEY` 变量

## 🚀 快速开始

### 1. 安装依赖
```bash
# 使用 uv 安装 Python 依赖
uv sync

# 或使用 pip
pip install -r requirements.txt
```

### 2. 配置环境变量
```bash
# 复制环境变量模板
cp .env.example .env

# 编辑 .env 文件，填入你的 API 密钥
nano .env
```

### 3. 启动服务器
```bash
# 启动 MCP 服务器
python server.py
```

### 4. 运行客户端
```bash
# 在另一个终端中运行客户端
python client.py server.py
```

### 5. 开始使用
```
你: 查询北京的天气
🤖 OpenAI: 我来为您查询北京的天气信息。

[Calling tool query_weather with args {'city': 'Beijing'}]

🤖 OpenAI: 根据查询结果，北京今天的天气情况如下：

📍 北京, CN
🌡️ 温度: 25°C
💧 湿度: 65%
💨 风速: 3.2 m/s
☁️ 天气: 多云
```

## 📁 项目结构

```
weather_mcp/
├── server.py          # MCP 服务器主文件
├── client.py          # MCP 客户端主文件
├── main.py            # 项目入口文件
├── config.py          # 配置文件
├── pyproject.toml     # Python 项目配置
├── package.json       # Node.js 项目配置
├── .env               # 环境变量文件
├── .gitignore         # Git 忽略文件
├── README.md          # 项目说明文档
└── requirements.txt   # Python 依赖列表
```

## 🔍 API 文档

### 可用工具

#### query_weather
- **描述**: 查询指定城市的天气信息
- **参数**: 
  - `city` (string): 城市名称（英文）
- **返回**: 格式化的天气信息字符串

### 使用示例

```python
# 查询天气
result = await session.call_tool("query_weather", {"city": "Shanghai"})
print(result.content[0].text)
```

## 🛠️ 开发指南

### 添加新的天气工具

1. 在 `server.py` 中添加新的工具函数
2. 使用 `@mcp.tool()` 装饰器注册工具
3. 实现工具逻辑和错误处理
4. 更新文档和测试

### 自定义配置

可以通过修改 `config.py` 文件来自定义服务配置：

```python
# 自定义天气 API 配置
WEATHER_CONFIG = {
    "api_base": "https://api.openweathermap.org/data/2.5/weather",
    "timeout": 30.0,
    "retry_count": 3
}
```

## 📝 许可证

本项目采用 MIT 许可证。详见 [LICENSE](LICENSE) 文件。

## 🤝 贡献

欢迎提交 Issue 和 Pull Request 来改进这个项目！

## 📞 支持

如果你遇到问题或有建议，请：
1. 查看 [Issues](https://github.com/LiuKeTong713/Weather_mcp/issues)
2. 创建新的 Issue
3. 联系项目维护者

---

**注意**: 请确保妥善保管你的 API 密钥，不要将其提交到版本控制系统中。
