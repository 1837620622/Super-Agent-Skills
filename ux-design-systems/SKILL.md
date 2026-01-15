---
name: ux-design-systems
description: "使用令牌、组件和主题构建一致的设计系统。适用于创建组件库、实现设计令牌、构建主题系统或确保设计一致性。触发关键词：design system, design tokens, component library, theming, dark mode, 设计系统, 组件库。"
---

# UX 设计系统

使用令牌和组件构建一致、可维护的设计系统。

## 设计令牌

### CSS 变量
```css
:root {
  /* 颜色 */
  --color-primary-50: #eff6ff;
  --color-primary-500: #3b82f6;
  --color-primary-900: #1e3a8a;
  
  /* 字体 */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  
  /* 间距 */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-4: 1rem;
  --space-8: 2rem;
  
  /* 边框圆角 */
  --radius-sm: 0.25rem;
  --radius-md: 0.375rem;
  --radius-lg: 0.5rem;
  --radius-full: 9999px;
  
  /* 阴影 */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1);
}
```

### TypeScript 令牌系统
```typescript
export const tokens = {
  colors: {
    primary: {
      50: '#eff6ff',
      500: '#3b82f6',
      900: '#1e3a8a',
    },
    gray: {
      50: '#f9fafb',
      500: '#6b7280',
      900: '#111827',
    },
  },
  spacing: {
    1: '0.25rem',
    2: '0.5rem',
    4: '1rem',
    8: '2rem',
  },
  fontSize: {
    sm: '0.875rem',
    base: '1rem',
    lg: '1.125rem',
    xl: '1.25rem',
  },
} as const;

type ColorToken = keyof typeof tokens.colors;
type SpaceToken = keyof typeof tokens.spacing;
```

## 组件模式

### 按钮组件
```tsx
import { cva, type VariantProps } from 'class-variance-authority';

const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-md font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        primary: 'bg-primary-500 text-white hover:bg-primary-600',
        secondary: 'bg-gray-100 text-gray-900 hover:bg-gray-200',
        outline: 'border border-gray-300 bg-transparent hover:bg-gray-50',
        ghost: 'hover:bg-gray-100',
        destructive: 'bg-red-500 text-white hover:bg-red-600',
      },
      size: {
        sm: 'h-8 px-3 text-sm',
        md: 'h-10 px-4 text-base',
        lg: 'h-12 px-6 text-lg',
      },
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md',
    },
  }
);

interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  isLoading?: boolean;
}

export function Button({
  className,
  variant,
  size,
  isLoading,
  children,
  ...props
}: ButtonProps) {
  return (
    <button
      className={buttonVariants({ variant, size, className })}
      disabled={isLoading}
      {...props}
    >
      {isLoading && <Spinner className="mr-2 h-4 w-4" />}
      {children}
    </button>
  );
}
```

## 深色模式

### 基于 CSS 的主题切换
```css
:root {
  --bg-primary: #ffffff;
  --text-primary: #111827;
  --border-color: #e5e7eb;
}

[data-theme='dark'] {
  --bg-primary: #111827;
  --text-primary: #f9fafb;
  --border-color: #374151;
}
```

### React 主题提供者
```tsx
import { createContext, useContext, useEffect, useState } from 'react';

type Theme = 'light' | 'dark' | 'system';

const ThemeContext = createContext<{
  theme: Theme;
  setTheme: (theme: Theme) => void;
}>({ theme: 'system', setTheme: () => {} });

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<Theme>('system');

  useEffect(() => {
    const root = document.documentElement;
    
    if (theme === 'system') {
      const systemTheme = window.matchMedia('(prefers-color-scheme: dark)').matches
        ? 'dark'
        : 'light';
      root.setAttribute('data-theme', systemTheme);
    } else {
      root.setAttribute('data-theme', theme);
    }
  }, [theme]);

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export const useTheme = () => useContext(ThemeContext);
```

## Tailwind CSS @theme 指令（v4+）

```css
/* 使用 @theme 指令自定义设计令牌 */
@import "tailwindcss";

@theme {
  /* 自定义字体 */
  --font-display: "Satoshi", "sans-serif";

  /* 自定义断点 */
  --breakpoint-xs: 30rem;
  --breakpoint-3xl: 120rem;

  /* 自定义颜色（使用 oklch 色彩空间） */
  --color-brand-100: oklch(0.99 0 0);
  --color-brand-200: oklch(0.98 0.04 113.22);
  --color-brand-300: oklch(0.94 0.11 115.03);
  --color-brand-400: oklch(0.92 0.19 114.08);
  --color-brand-500: oklch(0.84 0.18 117.33);
  --color-brand-600: oklch(0.53 0.12 118.34);

  /* 自定义缓动函数 */
  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
  --ease-snappy: cubic-bezier(0.2, 0, 0, 1);
}
```

### 在 HTML 中使用自定义令牌
```html
<div class="font-display text-brand-500 3xl:text-2xl transition-all ease-fluid">
  使用自定义设计令牌
</div>
```

## 深色模式与系统偏好

```css
/* 响应系统深色模式偏好 */
.card {
  @apply bg-white text-gray-900;
  @apply dark:bg-gray-800 dark:text-white;
}

/* 响应式 + 深色模式组合 */
.hero {
  @apply bg-gray-100 lg:bg-white;
  @apply lg:dark:bg-black;
}
```

```tsx
// 使用 prefers-color-scheme 媒体查询
function ThemeAwareComponent() {
  return (
    <div className="
      bg-white text-gray-900
      dark:bg-gray-900 dark:text-white
      lg:dark:bg-black
    ">
      自动适应系统主题偏好
    </div>
  );
}
```

## 相关资源

- **Tailwind CSS**: https://tailwindcss.com
- **Tailwind CSS v4**: https://tailwindcss.com/blog/tailwindcss-v4
- **CVA (类变体权威)**: https://cva.style
- **Tailwind Variants**: https://www.tailwind-variants.org/
