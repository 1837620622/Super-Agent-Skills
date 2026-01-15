---
name: bun
description: "使用 Bun JavaScript 运行时构建高性能应用。适用于创建 Bun 项目、使用 Bun API、打包、测试或替代 Node.js。触发关键词：Bun, Bun runtime, bun.sh, bunx, Bun serve, Bun test, JavaScript runtime。"
---

# Bun - 极速 JavaScript 运行时

使用 Bun 的一体化工具包构建和运行 JavaScript/TypeScript 应用。Bun 集运行时、打包器、测试运行器和包管理器于一身，提供显著的性能提升和 Node.js 兼容性。

## 快速开始

```bash
# 安装 Bun（macOS、Linux、WSL）
curl -fsSL https://bun.sh/install | bash

# Windows
powershell -c "irm bun.sh/install.ps1 | iex"

# 创建新项目
bun init

# 直接运行 TypeScript（无需编译步骤！）
bun run index.ts

# 安装依赖包（比 npm 更快）
bun install

# 运行脚本
bun run dev
```

## 包管理

```bash
# 安装依赖
bun install              # 从 package.json 安装所有依赖
bun add express          # 添加依赖
bun add -d typescript    # 添加开发依赖
bun add -g serve         # 添加全局包

# 移除包
bun remove express

# 更新包
bun update

# 运行包的可执行文件
bunx prisma generate     # 类似 npx 但更快
bunx create-next-app

# 锁定文件
bun install --frozen-lockfile  # CI 模式
```

### bun.lockb 与 package-lock.json
```bash
# Bun 使用二进制锁定文件（bun.lockb）- 速度更快
# 生成 yarn.lock 以保持兼容性：
bun install --yarn

# 从其他锁定文件导入
bun install  # 自动检测 package-lock.json、yarn.lock
```

## Bun 运行时

### 运行文件
```bash
# 运行任意文件
bun run index.ts         # TypeScript
bun run index.js         # JavaScript
bun run index.jsx        # JSX

# 监听模式
bun --watch run index.ts

# 热重载
bun --hot run server.ts
```

### 内置 API
```typescript
// --------------------------------------------------------------------------------
// 文件读写（超快速度）
// --------------------------------------------------------------------------------
const file = Bun.file('data.json');
const content = await file.text();    // 读取文本内容
const json = await file.json();       // 解析 JSON
const bytes = await file.arrayBuffer(); // 读取二进制数据

// 写入文件
await Bun.write('output.txt', 'Hello, Bun!');
await Bun.write('data.json', JSON.stringify({ key: 'value' }));
await Bun.write('image.png', await fetch('https://example.com/img.png'));

// 文件元数据
const fileInfo = Bun.file('data.json');
console.log(fileInfo.size);           // 文件大小（字节）
console.log(fileInfo.type);           // MIME 类型
console.log(fileInfo.lastModified);   // 最后修改时间

// Glob 文件匹配
const glob = new Bun.Glob('**/*.ts');
for await (const f of glob.scan('.')) {
  console.log(f);
}
```

### HTTP 服务器
```typescript
// --------------------------------------------------------------------------------
// 简单服务器：基础路由处理
// --------------------------------------------------------------------------------
const server = Bun.serve({
  port: 3000,
  fetch(req) {
    const url = new URL(req.url);
    
    if (url.pathname === '/') {
      return new Response('Hello, Bun!');
    }
    
    if (url.pathname === '/json') {
      return Response.json({ message: 'Hello!' });
    }
    
    return new Response('Not Found', { status: 404 });
  },
});

console.log(`服务器运行于 http://localhost:${server.port}`);
```

### 高级服务器
```typescript
// --------------------------------------------------------------------------------
// 高级服务器：支持 WebSocket、静态文件和 API 路由
// --------------------------------------------------------------------------------
Bun.serve({
  port: 3000,
  
  // 主请求处理器
  async fetch(req, server) {
    const url = new URL(req.url);
    
    // WebSocket 升级
    if (url.pathname === '/ws') {
      const upgraded = server.upgrade(req, {
        data: { userId: '123' },  // 附加数据到 socket
      });
      if (upgraded) return undefined;
    }
    
    // 静态文件服务
    if (url.pathname.startsWith('/static/')) {
      const filePath = `./public${url.pathname}`;
      const file = Bun.file(filePath);
      if (await file.exists()) {
        return new Response(file);
      }
    }
    
    // JSON API
    if (url.pathname === '/api/data' && req.method === 'POST') {
      const body = await req.json();
      return Response.json({ received: body });
    }
    
    return new Response('Not Found', { status: 404 });
  },
  
  // WebSocket 处理器
  websocket: {
    open(ws) {
      console.log('客户端已连接:', ws.data.userId);
      ws.subscribe('chat');  // 发布/订阅模式
    },
    message(ws, message) {
      // 广播给所有订阅者
      ws.publish('chat', message);
    },
    close(ws) {
      console.log('客户端已断开');
    },
  },
  
  // 错误处理
  error(error) {
    return new Response(`错误: ${error.message}`, { status: 500 });
  },
});
```

### WebSocket 客户端
```typescript
const ws = new WebSocket('ws://localhost:3000/ws');

ws.onopen = () => {
  ws.send('Hello, server!');
};

ws.onmessage = (event) => {
  console.log('收到消息:', event.data);
};
```

## Bun API

### SQLite（内置）
```typescript
import { Database } from 'bun:sqlite';

// --------------------------------------------------------------------------------
// SQLite 数据库：高性能嵌入式数据库
// --------------------------------------------------------------------------------
const db = new Database('mydb.sqlite');

// 创建表
db.exec(`
  CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    email TEXT UNIQUE
  )
`);

// 插入数据
const insert = db.prepare('INSERT INTO users (name, email) VALUES (?, ?)');
insert.run('Alice', 'alice@example.com');

// 查询单条记录
const query = db.prepare('SELECT * FROM users WHERE id = ?');
const user = query.get(1);

// 查询所有记录
const allUsers = db.prepare('SELECT * FROM users').all();

// 事务处理
const insertMany = db.transaction((users) => {
  for (const user of users) {
    insert.run(user.name, user.email);
  }
});

insertMany([
  { name: 'Bob', email: 'bob@example.com' },
  { name: 'Charlie', email: 'charlie@example.com' },
]);

// 将查询结果映射到类实例
class User {
  name!: string;
  email!: string;
  get domain() {
    return this.email.split("@")[1];
  }
}
const userQuery = db.query("SELECT * FROM users");
userQuery.as(User);
const userInstances = userQuery.all();
console.log(userInstances[0].domain);  // 输出邮箱域名

db.close();
```

### 密码哈希（内置）
```typescript
// 哈希密码
const hash = await Bun.password.hash('mypassword', {
  algorithm: 'argon2id',  // 或 'bcrypt'
  memoryCost: 65536,      // 64 MB
  timeCost: 2,
});

// 验证密码
const isValid = await Bun.password.verify('mypassword', hash);
```

### 进程管理
```typescript
// 启动进程
const proc = Bun.spawn(['ls', '-la'], {
  cwd: '/home/user',
  env: { ...process.env, MY_VAR: 'value' },
  stdout: 'pipe',
});

const output = await new Response(proc.stdout).text();
console.log(output);

// 同步执行
const result = Bun.spawnSync(['echo', 'hello']);
console.log(result.stdout.toString());

// Shell 命令
const { stdout } = Bun.spawn({
  cmd: ['sh', '-c', 'echo $HOME'],
  stdout: 'pipe',
});
```

### 哈希与加密
```typescript
// 快速哈希字符串（Wyhash 算法）
const hash = Bun.hash('hello world');

// 加密哈希
const sha256 = new Bun.CryptoHasher('sha256');
sha256.update('data');
const digest = sha256.digest('hex');

// 单行写法
const md5 = Bun.CryptoHasher.hash('md5', 'data', 'hex');

// HMAC
const hmac = Bun.CryptoHasher.hmac('sha256', 'secret-key', 'data', 'hex');
```

## 打包器

```bash
# 为浏览器打包
bun build ./src/index.ts --outdir ./dist

# 打包选项
bun build ./src/index.ts \
  --outdir ./dist \
  --minify \
  --sourcemap \
  --target browser \
  --splitting \
  --entry-naming '[dir]/[name]-[hash].[ext]'
```

### 打包 API
```typescript
// --------------------------------------------------------------------------------
// 程序化打包配置
// --------------------------------------------------------------------------------
const result = await Bun.build({
  entrypoints: ['./src/index.ts'],
  outdir: './dist',
  minify: true,
  sourcemap: 'external',
  target: 'browser',  // 'bun' | 'node' | 'browser'
  splitting: true,
  naming: {
    entry: '[dir]/[name]-[hash].[ext]',
    chunk: '[name]-[hash].[ext]',
    asset: '[name]-[hash].[ext]',
  },
  external: ['react', 'react-dom'],
  define: {
    'process.env.NODE_ENV': JSON.stringify('production'),
  },
  loader: {
    '.png': 'file',
    '.svg': 'text',
  },
});

if (!result.success) {
  console.error('打包失败:', result.logs);
}
```

## 测试

```typescript
// test.ts
import { describe, test, expect, beforeAll, afterAll, mock } from 'bun:test';

// --------------------------------------------------------------------------------
// 测试套件：数学运算
// --------------------------------------------------------------------------------
describe('数学运算', () => {
  test('加法', () => {
    expect(1 + 1).toBe(2);
  });
  
  test('数组包含', () => {
    expect([1, 2, 3]).toContain(2);
  });
  
  test('对象匹配', () => {
    expect({ name: 'Alice', age: 30 }).toMatchObject({ name: 'Alice' });
  });
  
  test('异步测试', async () => {
    const result = await Promise.resolve(42);
    expect(result).toBe(42);
  });
  
  test('抛出错误', () => {
    expect(() => {
      throw new Error('fail');
    }).toThrow('fail');
  });
});

// Mock 函数
const mockFn = mock(() => 'mocked');
mockFn();
expect(mockFn).toHaveBeenCalled();

// Mock 模块
mock.module('./database', () => ({
  query: mock(() => [{ id: 1 }]),
}));
```

```bash
# 运行测试
bun test

# 监听模式
bun test --watch

# 指定文件
bun test user.test.ts

# 代码覆盖率
bun test --coverage
```

## Node.js 兼容性

```typescript
// 大多数 Node.js API 开箱即用
import fs from 'fs';
import path from 'path';
import { createServer } from 'http';
import express from 'express';

// Bun 实现了 Node.js API
const data = fs.readFileSync('file.txt', 'utf-8');
const fullPath = path.join(__dirname, 'file.txt');

// Express 可以正常使用！
const app = express();
app.get('/', (req, res) => res.send('Hello!'));
app.listen(3000);
```

### Node.js 与 Bun API 对比
```typescript
// Node.js 方式
import { readFile } from 'fs/promises';
const content = await readFile('file.txt', 'utf-8');

// Bun 方式（更快）
const content = await Bun.file('file.txt').text();

// Node.js crypto
import crypto from 'crypto';
const hash = crypto.createHash('sha256').update('data').digest('hex');

// Bun 方式（更快）
const hash = Bun.CryptoHasher.hash('sha256', 'data', 'hex');
```

## 环境变量

```typescript
// 内置 .env 文件支持（无需 dotenv！）
// .env
// DATABASE_URL=postgres://localhost/db
// API_KEY=secret

// 访问环境变量
const dbUrl = Bun.env.DATABASE_URL;
const apiKey = process.env.API_KEY;  // 同样有效

// bunfig.toml 用于 Bun 配置
// [run]
// preload = ["./setup.ts"]
```

## HTTP 客户端

```typescript
// Fetch（在 Bun 中优化过）
const response = await fetch('https://api.example.com/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token',
  },
  body: JSON.stringify({ key: 'value' }),
});

const data = await response.json();

// 流式响应
const streamResponse = await fetch('https://api.example.com/stream');
const reader = streamResponse.body?.getReader();

while (true) {
  const { done, value } = await reader!.read();
  if (done) break;
  console.log(new TextDecoder().decode(value));
}
```

## 基于路由的 REST API 服务器

Bun 支持声明式路由配置：

```typescript
import { Database } from "bun:sqlite";

// --------------------------------------------------------------------------------
// 初始化数据库
// --------------------------------------------------------------------------------
const db = new Database("posts.db");
db.exec(`
  CREATE TABLE IF NOT EXISTS posts (
    id TEXT PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL,
    created_at TEXT NOT NULL
  )
`);

// --------------------------------------------------------------------------------
// REST API 服务器：声明式路由
// --------------------------------------------------------------------------------
Bun.serve({
  routes: {
    // 列出所有文章
    "/api/posts": {
      GET: () => {
        const posts = db.query("SELECT * FROM posts").all();
        return Response.json(posts);
      },

      // 创建文章
      POST: async req => {
        const post = await req.json();
        const id = crypto.randomUUID();

        db.query(
          `INSERT INTO posts (id, title, content, created_at)
           VALUES (?, ?, ?, ?)`
        ).run(id, post.title, post.content, new Date().toISOString());

        return Response.json({ id, ...post }, { status: 201 });
      }
    },

    // 根据 ID 获取文章
    "/api/posts/:id": req => {
      const post = db.query("SELECT * FROM posts WHERE id = ?").get(req.params.id);

      if (!post) {
        return new Response("Not Found", { status: 404 });
      }

      return Response.json(post);
    }
  },

  error(error) {
    console.error(error);
    return new Response("Internal Server Error", { status: 500 });
  }
});
```

## 项目结构

```
my-bun-project/
├── src/
│   ├── index.ts          # 入口文件
│   ├── routes/
│   │   └── api.ts
│   └── lib/
│       └── database.ts
├── tests/
│   └── index.test.ts
├── public/
│   └── 静态文件
├── package.json
├── bunfig.toml           # Bun 配置（可选）
├── tsconfig.json
└── .env
```

### bunfig.toml
```toml
[install]
# 默认使用精确版本
exact = true

# 注册表
registry = "https://registry.npmjs.org"

[run]
# 在 `bun run` 之前运行的脚本
preload = ["./instrumentation.ts"]

[test]
# 测试配置
coverage = true
coverageDir = "coverage"

[bundle]
# 默认打包配置
minify = true
sourcemap = "external"
```

## 性能对比

| 操作 | Node.js | Bun | 提升倍数 |
|------|---------|-----|----------|
| 启动时间 | ~40ms | ~7ms | 5.7x |
| 包安装 | ~10s | ~1s | 10x |
| 文件读取 | 基准 | 更快 | 10x |
| HTTP 服务器 | 基准 | 更快 | 4x |
| SQLite | 需外部库 | 内置 | 3x |
| TypeScript | 需编译 | 原生支持 | ∞ |

## 相关资源

- **Bun 官方文档**: https://bun.sh/docs
- **Bun API 参考**: https://bun.sh/docs/api
- **Bun Discord**: https://bun.sh/discord
- **GitHub**: https://github.com/oven-sh/bun
