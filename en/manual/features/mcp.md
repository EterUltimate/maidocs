---
title: MCP Tools Integration
---

# MCP Tools Integration 🔌

MaiBot not only can chat, it can also "do things"! Through MCP (Model Context Protocol), MaiBot can connect to various external tools, get real-time information, and complete various practical tasks.

## What is MCP?

MCP is like giving AI "hands and feet," allowing it to:
- **Search for information** - Search the web
- **Get data** - Check weather, stocks, news
- **Operate files** - Read/write files, process documents
- **Call services** - Use various online services

In simple terms, it transforms MaiBot from "only can talk" to "both talk and do."

## What can it do?

### 🌤️ Getting Real-time Information
**Weather Query**
```
You: How's the weather in Beijing tomorrow?
MaiBot: Let me check... Beijing will be sunny tomorrow,
       with temperatures of 15-25°C, great for going out!
```

**News Information**
```
You: Any tech news today?
MaiBot: Let me see... There are a few big news today:
       1. A major company released a new AI model
       2. New energy vehicle sales hit a record high
       3. SpaceX has another launch plan
```

**Stock Quotes**
```
You: Check Maotai stock price for me
MaiBot: Kweichow Moutai current price is 1678.50 yuan,
       up 2.3% today, doing well!
```

### 🔍 Search Queries
**Web Search**
```
You: What is quantum computing?
MaiBot: Let me search... Quantum computing is
       a computing method that uses quantum mechanics principles,
       it can... (gives detailed explanation)
```

**Encyclopedia Query**
```
You: Tell me about Miyazaki Hayao
MaiBot: Hayao Miyazaki is a famous Japanese animation director,
       born in 1941, famous works include "Spirited Away",
       "My Neighbor Totoro", "Castle in the Sky"...
```

### 📄 File Processing
**Document Reading**
```
You: Help me see what this PDF is about
MaiBot: This is a paper about machine learning,
       mainly about... (summarizes content)
```

**Data Analysis**
```
You: Analyze this Excel sheet
MaiBot: Based on data analysis, I found:
       1. Sales are continuously growing
       2. Q3 performed best
       3. Suggest paying attention to... (gives analysis)
```

### 🛠️ Practical Tools
**Calculator**
```
You: Help me calculate 15% of 2880
MaiBot: 15% of 2880 = 432
```

**Unit Conversion**
```
You: How many Celsius is 100 Fahrenheit?
MaiBot: 100°F = 37.78°C
```

**Time Query**
```
You: What time is it in New York?
MaiBot: It's currently 10:30 AM in New York
       (13 hours behind Beijing time)
```

## How does it work?

### 🔗 Connecting Tools
MaiBot connects to various tool services through the MCP protocol:
- **Search Engines** - Get latest information
- **Weather Services** - Query weather data
- **Financial Data** - Get stock quotes
- **Encyclopedia Knowledge** - Query professional information

### 🧠 Smart Invocation
When you ask questions, it:
1. **Analyzes the question** - Determines what tool is needed
2. **Calls the tool** - Connects to the corresponding service
3. **Gets the data** - Obtains the needed information
4. **Organizes the answer** - Replies in natural language

### ⚡ Real-time Response
- Tool calls are real-time
- Data is always the latest
- Response speed is fast

## Actual Usage Scenarios

### 📊 Work Assistant**
```
You: Help me check the recent financing situation in the AI industry
MaiBot: I searched, recent AI industry financing is very active:
       - An AI company got $1 billion in Series B funding
       - Focused on large language model R&D
       - Investors include... (detailed data)
```

### 🎓 Learning Helper
```
You: I want to learn about blockchain technology
MaiBot: Let me introduce you:
       Blockchain is a distributed ledger technology,
       its core characteristics are... (detailed explanation)
       Currently mainly applied in... (practical applications)
```

### 🏠 Life Assistant
```
You: Is tomorrow good for washing my car?
MaiBot: I checked the weather forecast, there's rain tomorrow,
       not recommended to wash your car. The day after is sunny,
       much better for washing your car!
```

## What tools are supported?

### 🔍 Search Types
- **Web Search** - Search internet information
- **Encyclopedia Query** - Query encyclopedia knowledge
- **News Search** - Get latest news

### 📊 Data Types
- **Weather Query** - Get weather information
- **Stock Query** - View stock quotes
- **Currency Conversion** - Query exchange rates

### 🛠️ Tool Types
- **Calculator** - Math calculations
- **Unit Conversion** - Various unit conversions
- **Time Query** - Time around the world

### 📄 File Types
- **Document Reading** - Read various documents
- **Data Analysis** - Process spreadsheet data
- **Text Processing** - Various text operations

## How to configure?

### ⚙️ Simple Configuration
Add in configuration file:
```toml
[mcp]
enable = true

# Add the tool services you want to use
[[mcp.servers]]
name = "search"
command = "search-server"
```

### 🔧 Advanced Settings
- Can configure multiple tool services
- Supports custom tools
- Can set usage permissions

## Usage Tips

### 💡 Asking Tips
- Ask specific questions
- Explain what information you need
- Can ask it to look up information

### 🎯 Usage Scenarios
- When you need real-time information
- When you need professional data
- When you need calculations or conversions

### ⚠️ Notes
- Tools need internet connection
- Some services may need API keys
- Response speed depends on tool service

## Extension Capabilities

### 🔌 Custom Tools
Developers can:
- Develop their own tools
- Connect to private services
- Customize special features

### 🌐 Third-party Integration
Supports connecting to:
- Enterprise internal services
- Professional databases
- Custom business systems

---

MCP transforms MaiBot from a "chatbot" to an "intelligent assistant," not only able to talk but also help you look up information, do calculations, and get information. It's like giving your AI companion "clairvoyance" and "keen hearing," letting it access a broader world of information!