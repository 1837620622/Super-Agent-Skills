<p align="center">
  <img src="https://img.shields.io/badge/🚀-Super%20Agent%20Skills-blueviolet?style=for-the-badge" alt="Super Agent Skills"/>
</p>

<h1 align="center">🧠 Super Agent Skills</h1>

<p align="center">
  <strong>为 AI 编程助手打造的专业技能库</strong><br>
  <em>让你的 AI 助手更智能、更专业、更高效</em>
</p>

<p align="center">
  <a href="./README.md">🇨🇳 中文</a> •
  <a href="./README_EN.md">🇺🇸 English</a>
</p>

<p align="center">
  <a href="#-特性">特性</a> •
  <a href="#-技能列表">技能列表</a> •
  <a href="#-安装指南">安装</a> •
  <a href="#-使用方法">使用</a> •
  <a href="#-贡献">贡献</a>
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/1837620622/Super-Agent-Skills?style=social" alt="Stars"/>
  <img src="https://img.shields.io/github/forks/1837620622/Super-Agent-Skills?style=social" alt="Forks"/>
  <img src="https://img.shields.io/github/license/1837620622/Super-Agent-Skills" alt="License"/>
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome"/>
</p>

---

## ✨ 特性

- 🎯 **专业深度** - 每个技能都基于官方文档和最佳实践精心编写
- 🔄 **持续更新** - 紧跟最新技术动态，定期更新内容
- 🌍 **中文友好** - 全中文注释和说明，降低学习门槛
- 🛠️ **即插即用** - 简单配置即可在主流 AI IDE 中使用
- 📚 **代码示例** - 丰富的可运行代码示例

---

## 📦 技能列表

### ☁️ 云平台与部署

| 技能 | 描述 | 关键功能 |
|------|------|----------|
| **Cloudflare** | Cloudflare Workers 边缘计算平台 | Workers、D1、R2、KV、AI、Vectorize、Workflows、Hyperdrive |
| **Vercel** | 现代前端部署平台 | Serverless、Edge Functions、KV、Postgres、Blob、AI SDK |
| **Railway** | 简化的云部署平台 | 容器部署、数据库、私有网络、多区域扩展 |

### 📄 文档处理

| 技能 | 描述 | 关键功能 |
|------|------|----------|
| **docx** | Word 文档处理 | python-docx、表格、样式、页眉页脚、修订追踪 |
| **xlsx** | Excel 电子表格处理 | openpyxl、公式、图表、条件格式、数据验证 |

### 🔬 科学计算

| 技能 | 描述 | 关键功能 |
|------|------|----------|
| **scientific-simulation** | 科学仿真与数值计算 | scipy solve_ivp、ODE求解、事件检测、刚性方程 |

### 🎨 更多技能

| 技能 | 描述 |
|------|------|
| **analytics-metrics** | 数据可视化与分析仪表盘 |
| **mongodb** | MongoDB 数据库操作 |
| **langchain** | LLM 应用与 Agent 开发 |
| **figma** | Figma API 与设计系统集成 |
| **owasp-security** | OWASP 安全编码实践 |
| **web-accessibility** | WCAG 无障碍开发指南 |
| **mermaid-diagrams** | Mermaid 图表生成 |
| **mobile-responsiveness** | 响应式移动端开发 |
| **ux-design-systems** | 设计系统与组件库 |

---

## 🚀 安装指南

### 方式一：直接下载

```bash
git clone https://github.com/1837620622/Super-Agent-Skills.git
```

### 方式二：手动复制

下载仓库中的 `skills` 文件夹，复制到对应 IDE 的配置目录。

---

## 💻 使用方法

### Windsurf (Codeium)

1. 打开 Windsurf 配置目录：
   ```
   # macOS
   ~/.codeium/windsurf/skills/
   
   # Windows
   %USERPROFILE%\.codeium\windsurf\skills\
   
   # Linux
   ~/.codeium/windsurf/skills/
   ```

2. 将下载的技能文件夹复制到该目录

3. 重启 Windsurf，技能将自动加载

### Cursor

1. 打开 Cursor 设置 (`Cmd/Ctrl + ,`)

2. 搜索 `Rules for AI` 或进入 **Features > Rules for AI**

3. 在规则区域添加技能内容，或引用技能文件：
   ```
   # 方式一：直接粘贴技能内容
   
   # 方式二：创建 .cursorrules 文件
   将技能内容复制到项目根目录的 .cursorrules 文件中
   ```

4. 技能将在对话中自动生效

### GitHub Copilot

1. 在项目根目录创建 `.github/copilot-instructions.md` 文件

2. 将需要的技能内容复制到该文件

3. Copilot 将根据指令提供更精准的建议

### 其他 AI IDE

大多数 AI 编程助手都支持自定义提示词或系统指令：

- **Cody (Sourcegraph)**: 在设置中添加自定义指令
- **Tabnine**: 通过 Team Settings 配置
- **Amazon CodeWhisperer**: 使用注释引导

---

## 📖 技能详情

### Cloudflare 技能亮点

```typescript
// Workers + D1 + AI 的完美结合
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const { results } = await env.DB.prepare('SELECT * FROM users').all();
    const response = await env.AI.run('@cf/meta/llama-3.1-8b-instruct', {
      messages: [{ role: 'user', content: '分析这些用户数据' }],
    });
    return Response.json({ users: results, analysis: response });
  },
};
```

### Vercel AI SDK 流式响应

```typescript
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();
  const result = streamText({
    model: openai('gpt-4o'),
    messages,
  });
  return result.toDataStreamResponse();
}
```

### Railway 多区域部署

```json
{
  "deploy": {
    "multiRegionConfig": {
      "us-west2": { "numReplicas": 2 },
      "europe-west4": { "numReplicas": 2 },
      "asia-southeast1": { "numReplicas": 2 }
    }
  }
}
```

---

## 🤝 贡献

欢迎提交 PR 来改进或添加新技能！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingSkill`)
3. 提交更改 (`git commit -m 'Add some AmazingSkill'`)
4. 推送到分支 (`git push origin feature/AmazingSkill`)
5. 开启 Pull Request

---

## 📬 联系作者

- **微信**: 1837620622（传康 kk）
- **邮箱**: 2040168455@qq.com
- **闲鱼/B站**: 万能程序员

---

## ⭐ 支持项目

如果这个项目对你有帮助，请给它一个 ⭐ Star！

你的支持是我持续更新的动力 💪

<p align="center">
  <a href="https://github.com/1837620622/Super-Agent-Skills">
    <img src="https://img.shields.io/badge/⭐-Star%20This%20Repo-yellow?style=for-the-badge" alt="Star"/>
  </a>
</p>

---

<p align="center">
  <sub>Made with ❤️ by 传康 kk</sub>
</p>
