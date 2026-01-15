<p align="center">
  <img src="https://img.shields.io/badge/🚀-Super%20Agent%20Skills-blueviolet?style=for-the-badge" alt="Super Agent Skills"/>
</p>

<h1 align="center">🧠 Super Agent Skills</h1>

<p align="center">
  <strong>Professional Skill Library for AI Programming Assistants</strong><br>
  <em>Make your AI assistant smarter, more professional, and more efficient</em>
</p>

<p align="center">
  <a href="./README.md">🇨🇳 中文</a> •
  <a href="./README_EN.md">🇺🇸 English</a>
</p>

<p align="center">
  <a href="#-features">Features</a> •
  <a href="#-skill-list">Skills</a> •
  <a href="#-installation">Installation</a> •
  <a href="#-usage">Usage</a> •
  <a href="#-contributing">Contributing</a>
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/1837620622/Super-Agent-Skills?style=social" alt="Stars"/>
  <img src="https://img.shields.io/github/forks/1837620622/Super-Agent-Skills?style=social" alt="Forks"/>
  <img src="https://img.shields.io/github/license/1837620622/Super-Agent-Skills" alt="License"/>
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome"/>
</p>

---

## ✨ Features

- 🎯 **Professional Depth** - Each skill is carefully crafted based on official documentation and best practices
- 🔄 **Continuously Updated** - Keeping up with the latest technology trends
- 🌍 **Bilingual Support** - Full Chinese and English documentation
- 🛠️ **Plug and Play** - Simple configuration for mainstream AI IDEs
- 📚 **Code Examples** - Rich runnable code examples

---

## 📦 Skill List

### ☁️ Cloud Platforms & Deployment

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **Cloudflare** | Cloudflare Workers Edge Computing | Workers, D1, R2, KV, AI, Vectorize, Workflows, Hyperdrive |
| **Vercel** | Modern Frontend Deployment | Serverless, Edge Functions, KV, Postgres, Blob, AI SDK |
| **Railway** | Simplified Cloud Deployment | Container Deploy, Databases, Private Network, Multi-region Scaling |

### 📄 Document Processing

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **docx** | Word Document Processing | python-docx, Tables, Styles, Headers/Footers, Track Changes |
| **xlsx** | Excel Spreadsheet Processing | openpyxl, Formulas, Charts, Conditional Formatting, Data Validation |

### 🔬 Scientific Computing

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **scientific-simulation** | Scientific Simulation & Numerical Computing | scipy solve_ivp, ODE Solving, Event Detection, Stiff Equations |

### 🎨 More Skills

| Skill | Description |
|-------|-------------|
| **analytics-metrics** | Data Visualization & Analytics Dashboards |
| **mongodb** | MongoDB Database Operations |
| **langchain** | LLM Applications & Agent Development |
| **figma** | Figma API & Design System Integration |
| **owasp-security** | OWASP Security Coding Practices |
| **web-accessibility** | WCAG Accessibility Guidelines |
| **mermaid-diagrams** | Mermaid Diagram Generation |
| **mobile-responsiveness** | Responsive Mobile Development |
| **ux-design-systems** | Design Systems & Component Libraries |

---

## 🚀 Installation

### Option 1: Clone Repository

```bash
git clone https://github.com/1837620622/Super-Agent-Skills.git
```

### Option 2: Manual Copy

Download the `skills` folder and copy to your IDE's configuration directory.

---

## 💻 Usage

### Windsurf (Codeium)

1. Open Windsurf configuration directory:
   ```
   # macOS
   ~/.codeium/windsurf/skills/
   
   # Windows
   %USERPROFILE%\.codeium\windsurf\skills\
   
   # Linux
   ~/.codeium/windsurf/skills/
   ```

2. Copy the downloaded skill folders to this directory

3. Restart Windsurf, skills will be automatically loaded

### Cursor

1. Open Cursor Settings (`Cmd/Ctrl + ,`)

2. Search for `Rules for AI` or navigate to **Features > Rules for AI**

3. Add skill content to the rules area, or reference skill files:
   ```
   # Option 1: Paste skill content directly
   
   # Option 2: Create .cursorrules file
   Copy skill content to .cursorrules file in project root
   ```

4. Skills will take effect automatically in conversations

### GitHub Copilot

1. Create `.github/copilot-instructions.md` file in project root

2. Copy desired skill content to this file

3. Copilot will provide more accurate suggestions based on instructions

### Other AI IDEs

Most AI programming assistants support custom prompts or system instructions:

- **Cody (Sourcegraph)**: Add custom instructions in settings
- **Tabnine**: Configure via Team Settings
- **Amazon CodeWhisperer**: Use comments as guides

---

## 📖 Skill Highlights

### Cloudflare - Workers + D1 + AI

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const { results } = await env.DB.prepare('SELECT * FROM users').all();
    const response = await env.AI.run('@cf/meta/llama-3.1-8b-instruct', {
      messages: [{ role: 'user', content: 'Analyze this user data' }],
    });
    return Response.json({ users: results, analysis: response });
  },
};
```

### Vercel AI SDK Streaming

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

### Railway Multi-Region Deployment

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

## 🤝 Contributing

PRs are welcome to improve or add new skills!

1. Fork this repository
2. Create feature branch (`git checkout -b feature/AmazingSkill`)
3. Commit changes (`git commit -m 'Add some AmazingSkill'`)
4. Push to branch (`git push origin feature/AmazingSkill`)
5. Open Pull Request

---

## 📬 Contact

- **WeChat**: 1837620622 (ChuanKang kk)
- **Email**: 2040168455@qq.com
- **Bilibili**: 万能程序员

---

## ⭐ Support

If this project helps you, please give it a ⭐ Star!

Your support motivates continuous updates 💪

<p align="center">
  <a href="https://github.com/1837620622/Super-Agent-Skills">
    <img src="https://img.shields.io/badge/⭐-Star%20This%20Repo-yellow?style=for-the-badge" alt="Star"/>
  </a>
</p>

---

<p align="center">
  <sub>Made with ❤️ by ChuanKang kk</sub>
</p>
