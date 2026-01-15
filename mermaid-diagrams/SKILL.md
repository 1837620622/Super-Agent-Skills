---
name: mermaid-diagrams
description: "使用 Mermaid 语法创建图表和可视化。适用于生成流程图、序列图、类图、实体关系图、甘特图或任何可视化文档。触发关键词：Mermaid, flowchart, sequence diagram, class diagram, ER diagram, Gantt chart, diagram, visualization, 流程图, 时序图。"
---

# Mermaid 图表

使用 Mermaid 语法创建文档和可视化图表。Mermaid 是一种基于文本的图表绘制工具，可以在 Markdown 中直接嵌入图表定义。

## 快速参考

Mermaid 图表使用 `mermaid` 语言标识符的 Markdown 代码块编写。

## 流程图

```mermaid
flowchart TD
    A[开始] --> B{是否有效?}
    B -->|是| C[处理数据]
    B -->|否| D[显示错误]
    C --> E[保存结果]
    D --> F[记录错误]
    E --> G[结束]
    F --> G
```

### 流程图语法
```
flowchart TD          %% TD = top-down, LR = left-right, RL, BT
    A[矩形]      %% Square brackets = rectangle
    B(圆角矩形)        %% Parentheses = rounded rectangle
    C{菱形}        %% Curly braces = diamond/decision
    D[[子程序]]   %% Double brackets = subroutine
    E[(数据库)]     %% Cylinder shape
    F((圆形))       %% Double parentheses = circle
    G>不对称]     %% Flag shape

    A --> B           %% Arrow
    B --- C           %% Line without arrow
    C -.-> D          %% Dotted arrow
    D ==> E           %% Thick arrow
    E --text--> F     %% Arrow with label
    F -->|label| G    %% Alternative label syntax
```

### 子图
```mermaid
flowchart TB
    subgraph 前端
        A[React 应用] --> B[组件]
        B --> C[Hooks]
    end
    
    subgraph 后端
        D[API 服务器] --> E[数据库]
    end
    
    A -->|HTTP| D
```

## 序列图

```mermaid
sequenceDiagram
    participant U as 用户
    participant C as 客户端
    participant S as 服务器
    participant D as 数据库

    U->>C: 点击提交
    C->>S: POST /api/data
    activate S
    S->>D: INSERT 查询
    D-->>S: 成功
    S-->>C: 200 OK
    deactivate S
    C-->>U: 显示成功消息
```

### 序列图语法
```
sequenceDiagram
    participant A as 张三
    participant B as 李四

    A->>B: 实线带箭头
    A-->>B: 虚线带箭头
    A-)B: 实线开放箭头
    A--)B: 虚线开放箭头
    
    activate B          %% 激活框
    B->>A: 响应
    deactivate B
    
    Note over A,B: 这是一个备注
    Note right of A: 右侧备注
    
    alt 条件为真
        A->>B: 执行这个
    else 条件为假
        A->>B: 执行那个
    end
    
    loop 每分钟
        A->>B: 心跳检测
    end
    
    opt 可选操作
        A->>B: 可能执行这个
    end
```

## 类图

```mermaid
classDiagram
    class User {
        +String id
        +String name
        +String email
        +login()
        +logout()
    }
    
    class Order {
        +String id
        +Date createdAt
        +calculateTotal()
    }
    
    class Product {
        +String id
        +String name
        +Number price
    }
    
    User "1" --> "*" Order : 下单
    Order "*" --> "*" Product : 包含
```

### 类图语法
```
classDiagram
    class 类名 {
        +公有字段
        -私有字段
        #受保护字段
        ~包级字段
        +公有方法()
        -私有方法()
    }
    
    ClassA <|-- ClassB : 继承
    ClassC *-- ClassD : 组合
    ClassE o-- ClassF : 聚合
    ClassG --> ClassH : 关联
    ClassI ..> ClassJ : 依赖
    ClassK ..|> ClassL : 实现
```

## 实体关系图

```mermaid
erDiagram
    USER ||--o{ ORDER : 下单
    ORDER ||--|{ LINE_ITEM : 包含
    PRODUCT ||--o{ LINE_ITEM : "属于"
    
    USER {
        string id PK
        string email UK
        string name
        datetime created_at
    }
    
    ORDER {
        string id PK
        string user_id FK
        datetime created_at
        string status
    }
    
    PRODUCT {
        string id PK
        string name
        decimal price
    }
    
    LINE_ITEM {
        string id PK
        string order_id FK
        string product_id FK
        int quantity
    }
```

### ER 图基数符号
```
||--||   一对一
||--o{   一对零或多
||--|{   一对一或多
}o--o{   零或多对零或多
```

## 甘特图

```mermaid
gantt
    title 项目时间线
    dateFormat YYYY-MM-DD
    
    section 规划阶段
    需求分析        :a1, 2024-01-01, 7d
    设计            :a2, after a1, 14d
    
    section 开发阶段
    后端 API        :b1, after a2, 21d
    前端            :b2, after a2, 28d
    集成            :b3, after b1, 7d
    
    section 测试阶段
    QA 测试         :c1, after b3, 14d
    Bug 修复        :c2, after c1, 7d
    
    section 发布
    部署            :milestone, after c2, 0d
```

## 状态图

```mermaid
stateDiagram-v2
    [*] --> 空闲
    空闲 --> 处理中: 提交
    处理中 --> 成功: 有效
    处理中 --> 错误: 无效
    成功 --> 空闲: 重置
    错误 --> 空闲: 重试
    成功 --> [*]
```

## 饼图

```mermaid
pie title 浏览器市场份额
    "Chrome" : 65
    "Safari" : 19
    "Firefox" : 10
    "Edge" : 4
    "其他" : 2
```

## Git 图

```mermaid
gitGraph
    commit
    commit
    branch feature
    checkout feature
    commit
    commit
    checkout main
    merge feature
    commit
    branch hotfix
    checkout hotfix
    commit
    checkout main
    merge hotfix
```

## 用户旅程图

```mermaid
journey
    title 用户结账体验
    section 浏览
        查看商品: 5: 用户
        添加到购物车: 4: 用户
    section 结账
        输入收货地址: 3: 用户
        输入支付信息: 2: 用户
        确认订单: 5: 用户
    section 购买后
        收到确认信息: 5: 用户, 系统
        跟踪物流: 4: 用户
```

## 思维导图

```mermaid
mindmap
    root((项目))
        前端
            React
            TypeScript
            Tailwind
        后端
            Node.js
            PostgreSQL
            Redis
        运维
            Docker
            Kubernetes
            CI/CD
```

## 样式定义

```mermaid
flowchart LR
    A[开始]:::green --> B[处理]:::blue --> C[结束]:::red
    
    classDef green fill:#22c55e,color:#fff
    classDef blue fill:#3b82f6,color:#fff
    classDef red fill:#ef4444,color:#fff
```

## React 组件

```tsx
import mermaid from 'mermaid';
import { useEffect, useRef } from 'react';

// --------------------------------------------------------------------------------
// Mermaid 初始化配置
// --------------------------------------------------------------------------------
mermaid.initialize({
  startOnLoad: true,
  theme: 'neutral', // default, dark, forest, neutral
  securityLevel: 'loose',
});

interface MermaidProps {
  chart: string;
  id?: string;
}

// --------------------------------------------------------------------------------
// Mermaid 组件：渲染 Mermaid 图表
// --------------------------------------------------------------------------------
export function Mermaid({ chart, id = 'mermaid-diagram' }: MermaidProps) {
  const ref = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (ref.current) {
      mermaid.render(id, chart).then(({ svg }) => {
        if (ref.current) {
          ref.current.innerHTML = svg;
        }
      });
    }
  }, [chart, id]);

  return <div ref={ref} className="mermaid-container" />;
}

// 使用示例
<Mermaid
  chart={`
    flowchart LR
      A --> B --> C
  `}
/>
```

## 使用技巧

1. **方向**: 使用 `TD`（从上到下）、`LR`（从左到右）、`BT`（从下到上）、`RL`（从右到左）
2. **注释**: 使用 `%%` 添加注释
3. **引号**: 对包含特殊字符的标签使用引号：`A["带括号的标签(parentheses)"]`
4. **换行**: 使用 `<br/>` 实现多行标签

## 相关资源

- **Mermaid 官方文档**: https://mermaid.js.org/
- **在线编辑器**: https://mermaid.live
- **GitHub 支持**: Mermaid 原生支持 GitHub Markdown
