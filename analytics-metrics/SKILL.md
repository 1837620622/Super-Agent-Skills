---
name: analytics-metrics
description: "构建数据可视化和分析仪表盘。适用于创建图表、KPI 展示、指标仪表盘或数据可视化组件。触发关键词：analytics, dashboard, charts, metrics, KPI, data visualization, Recharts, 数据可视化, 仪表盘, 图表。"
---

# 数据分析与指标仪表盘

使用 Recharts 构建数据可视化组件。Recharts 是基于 React 和 D3 构建的声明式图表库，支持 React 16.8+ 至 React 19，提供响应式容器、动画、自定义形状和无障碍功能。

## 快速开始

```bash
# 安装 Recharts 及其依赖
npm install recharts react-is
```

## 折线图

```tsx
import {
  LineChart,
  Line,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  Legend,
  ResponsiveContainer,
} from 'recharts';

// --------------------------------------------------------------------------------
// 数据定义：定义图表展示的数据结构
// --------------------------------------------------------------------------------
const data = [
  { month: 'Jan', revenue: 4000, users: 2400 },
  { month: 'Feb', revenue: 3000, users: 1398 },
  { month: 'Mar', revenue: 2000, users: 9800 },
  { month: 'Apr', revenue: 2780, users: 3908 },
];

// --------------------------------------------------------------------------------
// 收入图表组件：展示多数据系列的折线图
// --------------------------------------------------------------------------------
function RevenueChart() {
  return (
    <ResponsiveContainer width="100%" height={400}>
      <LineChart data={data}>
        <CartesianGrid strokeDasharray="3 3" />
        <XAxis dataKey="month" />
        <YAxis />
        <Tooltip />
        <Legend />
        <Line
          type="monotone"
          dataKey="revenue"
          stroke="#3b82f6"
          strokeWidth={2}
        />
        <Line
          type="monotone"
          dataKey="users"
          stroke="#10b981"
          strokeWidth={2}
        />
      </LineChart>
    </ResponsiveContainer>
  );
}
```

## 柱状图

```tsx
import { BarChart, Bar, XAxis, YAxis, Tooltip, ResponsiveContainer } from 'recharts';

// --------------------------------------------------------------------------------
// 销售图表组件：展示柱状图数据
// --------------------------------------------------------------------------------
function SalesChart({ data }) {
  return (
    <ResponsiveContainer width="100%" height={300}>
      <BarChart data={data}>
        <XAxis dataKey="name" />
        <YAxis />
        <Tooltip />
        <Bar dataKey="sales" fill="#3b82f6" radius={[4, 4, 0, 0]} />
      </BarChart>
    </ResponsiveContainer>
  );
}
```

## KPI 指标卡片

```tsx
// --------------------------------------------------------------------------------
// KPI 卡片属性接口定义
// --------------------------------------------------------------------------------
interface KPICardProps {
  title: string;           // 指标标题
  value: string | number;  // 指标数值
  change: number;          // 变化百分比
  trend: 'up' | 'down';    // 趋势方向
  icon?: React.ReactNode;  // 可选图标
}

// --------------------------------------------------------------------------------
// KPI 卡片组件：展示关键业务指标
// --------------------------------------------------------------------------------
function KPICard({ title, value, change, trend, icon }: KPICardProps) {
  const isPositive = trend === 'up';
  
  return (
    <div className="bg-white rounded-lg p-6 shadow-sm border">
      <div className="flex items-center justify-between">
        <p className="text-sm font-medium text-gray-500">{title}</p>
        {icon && <div className="text-gray-400">{icon}</div>}
      </div>
      
      <div className="mt-2">
        <p className="text-3xl font-semibold text-gray-900">{value}</p>
        
        <div className="mt-2 flex items-center">
          <span
            className={`text-sm font-medium ${
              isPositive ? 'text-green-600' : 'text-red-600'
            }`}
          >
            {isPositive ? '↑' : '↓'} {Math.abs(change)}%
          </span>
          <span className="ml-2 text-sm text-gray-500">较上期</span>
        </div>
      </div>
    </div>
  );
}
```

## 仪表盘布局

```tsx
// --------------------------------------------------------------------------------
// 仪表盘组件：整合 KPI 卡片和图表的完整仪表盘
// --------------------------------------------------------------------------------
function Dashboard() {
  return (
    <div className="p-6 space-y-6">
      {/* KPI 指标网格 */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        <KPICard
          title="总收入"
          value="$45,231"
          change={12.5}
          trend="up"
        />
        <KPICard
          title="活跃用户"
          value="2,345"
          change={8.2}
          trend="up"
        />
        <KPICard
          title="转化率"
          value="3.2%"
          change={-2.1}
          trend="down"
        />
        <KPICard
          title="平均订单金额"
          value="$142"
          change={5.4}
          trend="up"
        />
      </div>

      {/* 图表网格 */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <div className="bg-white rounded-lg p-6 shadow-sm border">
          <h3 className="text-lg font-medium mb-4">收入趋势</h3>
          <RevenueChart />
        </div>
        
        <div className="bg-white rounded-lg p-6 shadow-sm border">
          <h3 className="text-lg font-medium mb-4">分类销售</h3>
          <SalesChart data={salesData} />
        </div>
      </div>
    </div>
  );
}
```

## 饼图

```tsx
import { PieChart, Pie, Cell, Tooltip, Legend, ResponsiveContainer } from 'recharts';

// --------------------------------------------------------------------------------
// 颜色配置：定义饼图各扇区的颜色
// --------------------------------------------------------------------------------
const COLORS = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444'];

// --------------------------------------------------------------------------------
// 分布图表组件：展示数据占比分布的环形饼图
// --------------------------------------------------------------------------------
function DistributionChart({ data }) {
  return (
    <ResponsiveContainer width="100%" height={300}>
      <PieChart>
        <Pie
          data={data}
          cx="50%"
          cy="50%"
          innerRadius={60}
          outerRadius={100}
          dataKey="value"
          label
        >
          {data.map((_, index) => (
            <Cell key={index} fill={COLORS[index % COLORS.length]} />
          ))}
        </Pie>
        <Tooltip />
        <Legend />
      </PieChart>
    </ResponsiveContainer>
  );
}
```

## 组合图表（多类型混合）

Recharts v3.0+ 支持在 ComposedChart 中混合使用多种图表类型：

```tsx
import {
  ComposedChart,
  Area,
  Line,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  Legend,
  ResponsiveContainer,
} from 'recharts';

// --------------------------------------------------------------------------------
// 组合图表：在同一图表中展示面积图、折线图和柱状图
// --------------------------------------------------------------------------------
function CombinedChart({ data }) {
  return (
    <ResponsiveContainer width="100%" height={400}>
      <ComposedChart data={data}>
        <CartesianGrid strokeDasharray="3 3" />
        <XAxis dataKey="name" />
        <YAxis />
        <Tooltip />
        <Legend />
        <Area type="monotone" dataKey="amt" fill="#8884d8" stroke="#82ca9d" />
        <Line type="monotone" dataKey="uv" stroke="#ffc658" />
        <Bar dataKey="pv" barSize={20} fill="#413ea0" />
      </ComposedChart>
    </ResponsiveContainer>
  );
}
```

## 格式化工具函数

```typescript
// --------------------------------------------------------------------------------
// 数字格式化：将大数字转换为易读格式（K、M）
// --------------------------------------------------------------------------------
export function formatNumber(value: number): string {
  if (value >= 1_000_000) {
    return `${(value / 1_000_000).toFixed(1)}M`;
  }
  if (value >= 1_000) {
    return `${(value / 1_000).toFixed(1)}K`;
  }
  return value.toLocaleString();
}

// --------------------------------------------------------------------------------
// 货币格式化：将数字转换为货币格式
// --------------------------------------------------------------------------------
export function formatCurrency(value: number, currency = 'USD'): string {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency,
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(value);
}

// --------------------------------------------------------------------------------
// 百分比格式化：将数字转换为带符号的百分比
// --------------------------------------------------------------------------------
export function formatPercent(value: number): string {
  return `${value >= 0 ? '+' : ''}${value.toFixed(1)}%`;
}
```

## 常见使用场景

Recharts 适用于以下场景：
- **仪表盘**：展示 KPI 和混合图表类型
- **时间序列分析**：支持缩放和区域选择（Brush 组件）
- **对比分析**：堆叠柱状图或分组柱状图
- **占比展示**：饼图和环形图
- **径向进度**：RadialBar 组件
- **层级数据**：Treemap 和 Sunburst 图
- **流程可视化**：Sankey 图
- **转化漏斗**：Funnel 图

## 相关资源

- **Recharts 官方文档**: https://recharts.org
- **Recharts GitHub**: https://github.com/recharts/recharts
- **D3.js**: https://d3js.org
- **在线编辑器**: https://recharts.org/examples
