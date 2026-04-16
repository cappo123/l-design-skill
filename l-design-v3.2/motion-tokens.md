# Lanbow V3 — Motion Tokens & Principles

动效的基础规则与参数。**每次涉及动效时先读此文件**，具体实现 pattern 见 `motion-patterns.md`。

---

## MOTION PRINCIPLES

| 原则 | 说明 |
|---|---|
| **Purposeful** | 每个动效必须传达信息：确认操作、引导注意力、维持上下文 |
| **Restrained** | 匹配 Lanbow 克制的美学，避免花哨动画 |
| **Physics-based** | 优先使用 spring 动画而非线性，更自然 |
| **Accessible** | 始终尊重 `prefers-reduced-motion` |

---

## TIMING GUIDELINES

| Duration | Use Case |
|---|---|
| 100–150ms | Micro-feedback: hover, click, toggle |
| 200–300ms | Small transitions: dropdown, tooltip |
| 300–500ms | Medium transitions: modal, page change, card expand |
| 500ms+ | Complex choreography: page load stagger |

---

## EASING TOKENS

```css
:root {
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);      /* Decelerate — entering elements */
  --ease-in: cubic-bezier(0.55, 0, 1, 0.45);       /* Accelerate — exiting elements */
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);   /* Both — moving between states */
  --spring: cubic-bezier(0.34, 1.56, 0.64, 1);     /* Overshoot — playful/bouncy */
}
```

---

## FLOATING LAYER MOTION RULES

浮动层动效总原则：**快速、克制、无弹性过冲**。进入用 ease-out，退出用 ease-in，退出时长 ≤ 进入时长的 60-70%。

| 属性 | 进入 | 退出 |
|---|---|---|
| Duration | 200–300ms | 120–180ms |
| Easing | `--ease-out` | `--ease-in` |
| Transform origin | 靠近触发元素 | 同进入 |
| 只动 | `opacity` + `transform` | `opacity` + `transform` |

### 速查表

| 组件 | Shadow | 进入 | 退出 | Duration (in/out) |
|---|---|---|---|---|
| Dropdown Menu | `--shadow-md` | scaleY 0.92→1, y -4→0, fade in | 反向 | 200ms / 120ms |
| Popover | `--shadow-md` | scale 0.95→1, offset→0, fade in | 反向 | 200ms / 120ms |
| Modal | `--shadow-lg` | scale 0.95→1, y 12→0, fade in + backdrop | 反向 | 250ms / 150ms |
| Side Drawer | `--shadow-overlay` | x 100%→0 + backdrop | 反向 | 300ms / 200ms |
| Toast | `--shadow-md` | y 20→0, fade in | x→100, fade out | spring / spring |

---

## ACCESSIBILITY: REDUCED MOTION

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

```tsx
// React hook
function useReducedMotion() {
  const [prefersReduced, setPrefersReduced] = useState(false);
  useEffect(() => {
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    setPrefersReduced(mq.matches);
    const handler = (e: MediaQueryListEvent) => setPrefersReduced(e.matches);
    mq.addEventListener('change', handler);
    return () => mq.removeEventListener('change', handler);
  }, []);
  return prefersReduced;
}
```

---

## CSS KEYFRAME ANIMATIONS (Base)

```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(12px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.4; }
}

.animate-fadeInUp {
  animation: fadeInUp 0.4s var(--ease-out) both;
}
.animate-pulse {
  animation: pulse 1.8s ease-in-out infinite;
}

/* Staggered children */
.stagger > *:nth-child(1) { animation-delay: 0ms; }
.stagger > *:nth-child(2) { animation-delay: 80ms; }
.stagger > *:nth-child(3) { animation-delay: 160ms; }
.stagger > *:nth-child(4) { animation-delay: 240ms; }
.stagger > *:nth-child(5) { animation-delay: 320ms; }
```

---

## PERFORMANCE BEST PRACTICES

1. **只动 `transform` 和 `opacity`** — 保证 60fps，避免动 `width/height/top/left`
2. **谨慎使用 `will-change`** — 仅在确实需要 GPU 加速的元素上
3. **Spring > Linear** — 使用弹性动画更自然，匹配 Lanbow 的品质感
4. **可中断** — 允许用户在动画过程中操作，不要阻塞交互
5. **渐进增强** — 无 JS 时也能正常使用，动效是增强而非必需
