# Lanbow V3 — Microinteractions & Motion Design

Lanbow V3 的动效风格: **克制、精准、有目的**。符合设计系统的 restrained aesthetic，动效不是装饰而是反馈。

---

## MOTION PRINCIPLES

| 原则 | 说明 |
|---|---|
| **Purposeful** | 每个动效必须传达信息：确认操作、引导注意力、维持上下文 |
| **Restrained** | 匹配 Lanbow 克制的美学，避免花哨动画 |
| **Physics-based** | 优先使用 spring 动画而非线性，更自然 |
| **Accessible** | 始终尊重 `prefers-reduced-motion` |

## TIMING GUIDELINES

| Duration | Use Case |
|---|---|
| 100-150ms | Micro-feedback: hover, click, toggle |
| 200-300ms | Small transitions: dropdown, tooltip |
| 300-500ms | Medium transitions: modal, page change, card expand |
| 500ms+ | Complex choreography: page load stagger |

## EASING FUNCTIONS

```css
:root {
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);      /* Decelerate — entering elements */
  --ease-in: cubic-bezier(0.55, 0, 1, 0.45);       /* Accelerate — exiting elements */
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);   /* Both — moving between states */
  --spring: cubic-bezier(0.34, 1.56, 0.64, 1);     /* Overshoot — playful/bouncy */
}
```

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

## 1. BUTTON MICROINTERACTIONS

**Primary Button — Press Feedback:**
```tsx
import { motion } from 'framer-motion'

function LanbowButton({ children, onClick, variant = 'primary' }) {
  const styles = {
    primary: 'bg-grey-01 text-white',
    secondary: 'bg-transparent border border-grey-01 text-grey-01',
  }

  return (
    <motion.button
      onClick={onClick}
      whileHover={{ opacity: 0.85 }}
      whileTap={{ scale: 0.97 }}
      transition={{ type: 'spring', stiffness: 400, damping: 17 }}
      className={`px-6 py-2.5 font-ui text-sm font-medium ${styles[variant]}`}
      style={{ borderRadius: 0 }}
    >
      {children}
    </motion.button>
  )
}
```

**Loading Button — State Transition:**
```tsx
import { motion, AnimatePresence } from 'framer-motion'

function LoadingButton({ isLoading, children, onClick }) {
  return (
    <motion.button
      onClick={onClick}
      disabled={isLoading}
      whileTap={!isLoading ? { scale: 0.97 } : {}}
      className="relative px-6 py-2.5 bg-[var(--grey-01)] text-white font-ui text-sm overflow-hidden"
      style={{ borderRadius: 0 }}
    >
      <AnimatePresence mode="wait">
        {isLoading ? (
          <motion.span
            key="loading"
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -10 }}
            className="flex items-center gap-2"
          >
            <svg className="w-4 h-4 animate-spin" viewBox="0 0 24 24">
              <circle cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="3"
                fill="none" strokeDasharray="62.83" strokeDashoffset="15" strokeLinecap="round" />
            </svg>
            Processing...
          </motion.span>
        ) : (
          <motion.span
            key="idle"
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -10 }}
          >
            {children}
          </motion.span>
        )}
      </AnimatePresence>
    </motion.button>
  )
}
```

**"View All" Text Button — Underline Hover:**
```css
.btn-view-all {
  font-family: var(--font-ui);
  font-weight: 700;
  font-size: 10px;
  color: var(--grey-01);
  text-decoration: none;
  border: none;
  background: none;
  padding: 0 0 4px;
  border-bottom: 1px solid var(--grey-01);
  cursor: pointer;
  transition: opacity 0.15s var(--ease-out);
}
.btn-view-all:hover {
  opacity: 0.6;
}
```

---

## 2. CARD INTERACTIONS

**Card Hover — Subtle Lift (V3: no shadow, use translate only):**
```css
.card {
  background: var(--white);
  padding: var(--space-xl);
  border-radius: 0;
  transition: transform 0.2s var(--ease-out);
}
.card:hover {
  transform: translateY(-2px);
}
```

**Report Sub-Card — Click Feedback:**
```tsx
function ReportCardInteractive({ title, body, date, isNew, onClick }) {
  return (
    <motion.div
      onClick={onClick}
      whileHover={{ y: -2 }}
      whileTap={{ scale: 0.985 }}
      transition={{ type: 'spring', stiffness: 400, damping: 25 }}
      className="card-warm cursor-pointer"
      style={{
        background: 'var(--card-warm-grey)',
        padding: '16px 20px',
        borderRadius: 0,
        display: 'flex',
        flexDirection: 'column',
        justifyContent: 'space-between',
        minHeight: 200,
      }}
    >
      {/* card content */}
    </motion.div>
  )
}
```

---

## 3. PAGE LOAD — STAGGERED REVEAL

```tsx
import { motion } from 'framer-motion'

const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: { staggerChildren: 0.08, delayChildren: 0.1 },
  },
}

const itemVariants = {
  hidden: { opacity: 0, y: 16 },
  visible: {
    opacity: 1,
    y: 0,
    transition: { duration: 0.4, ease: [0.16, 1, 0.3, 1] },
  },
}

function DashboardPage() {
  return (
    <motion.div
      className="lanbow-container"
      variants={containerVariants}
      initial="hidden"
      animate="visible"
    >
      <motion.h1 variants={itemVariants} className="h1-title">
        Project Name
      </motion.h1>
      <motion.div variants={itemVariants} className="content-grid">
        {/* cards stagger in naturally */}
      </motion.div>
    </motion.div>
  )
}
```

---

## 4. SCROLL ANIMATIONS

**Fade-In on Scroll (Intersection Observer):**
```tsx
function useInView({ threshold = 0, triggerOnce = false } = {}) {
  const ref = useRef(null);
  const [isInView, setIsInView] = useState(false);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;
    const observer = new IntersectionObserver(
      ([entry]) => {
        const inView = entry.isIntersecting;
        setIsInView(inView);
        if (inView && triggerOnce) observer.unobserve(el);
      },
      { threshold }
    );
    observer.observe(el);
    return () => observer.disconnect();
  }, [threshold, triggerOnce]);

  return [ref, isInView];
}

function FadeInSection({ children }) {
  const [ref, isInView] = useInView({ threshold: 0.2, triggerOnce: true });
  return (
    <div
      ref={ref}
      className={`transition-all duration-700 ${
        isInView ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-8'
      }`}
    >
      {children}
    </div>
  );
}
```

**Scroll Progress Bar (Cyan accent):**
```tsx
import { motion, useScroll, useSpring } from 'framer-motion'

function ScrollProgress() {
  const { scrollYProgress } = useScroll();
  const scaleX = useSpring(scrollYProgress, {
    stiffness: 100, damping: 30, restDelta: 0.001,
  });
  return (
    <motion.div
      className="fixed top-0 left-0 right-0 h-[2px] origin-left z-50"
      style={{ scaleX, backgroundColor: 'var(--lanbow-cyan)' }}
    />
  );
}
```

---

## 5. NAVIGATION INTERACTIONS

**Top Nav — Active Link Indicator (opacity transition):**
```tsx
function LanbowNav({ items, activeItem }) {
  return (
    <nav className="lanbow-nav">
      <div className="nav-left">
        <div className="nav-logo">{/* Logo */}</div>
        <div className="nav-divider" />
        <div className="nav-links">
          {items.map((item) => (
            <motion.a
              key={item.href}
              href={item.href}
              className="nav-link"
              animate={{ opacity: item.id === activeItem ? 1 : 0.5 }}
              whileHover={{ opacity: 0.8 }}
              transition={{ duration: 0.15 }}
            >
              {item.label}
            </motion.a>
          ))}
        </div>
      </div>
    </nav>
  );
}
```

---

## 6. NOTIFICATION & FEEDBACK

**Toast Notification (V3 style: sharp corners, Grey-01 bg, shadow-md for floating):**
```tsx
import { motion, AnimatePresence } from 'framer-motion'

function ToastContainer({ toasts, onDismiss }) {
  return (
    <div className="fixed bottom-6 right-6 space-y-2 z-50">
      <AnimatePresence>
        {toasts.map((toast) => (
          <motion.div
            key={toast.id}
            initial={{ opacity: 0, y: 20, scale: 0.95 }}
            animate={{ opacity: 1, y: 0, scale: 1 }}
            exit={{ opacity: 0, x: 100, scale: 0.95 }}
            transition={{ type: 'spring', stiffness: 400, damping: 25 }}
            onClick={() => onDismiss(toast.id)}
            style={{
              background: 'var(--grey-01)',
              color: 'var(--white)',
              fontFamily: 'var(--font-ui)',
              fontSize: 13,
              padding: '12px 20px',
              borderRadius: 0,
              cursor: 'pointer',
              minWidth: 240,
              boxShadow: 'var(--shadow-md)',
            }}
          >
            {toast.message}
          </motion.div>
        ))}
      </AnimatePresence>
    </div>
  );
}
```

---

## 6.1 FLOATING LAYER ANIMATIONS (Dropdown / Popover / Modal / Side Drawer)

浮动层是用户交互中最高频的动效场景。V3 风格: **快速、克制、无弹性过冲**。进入用 ease-out（减速），退出用 ease-in（加速），退出时长 ≤ 进入时长的 60-70%。

### 通用原则

| 属性 | 进入 | 退出 |
|---|---|---|
| Duration | 200–300ms | 120–180ms |
| Easing | `--ease-out` | `--ease-in` |
| Transform origin | 靠近触发元素 | 同进入 |
| 只动 | `opacity` + `transform` | `opacity` + `transform` |

### Dropdown Menu

从触发按钮方向展开，默认向下。Shadow: `--shadow-md`。

**CSS 方案:**
```css
.dropdown-menu {
  position: absolute;
  z-index: 50;
  background: var(--white);
  border: 1px solid var(--stroke);
  border-radius: 0;
  box-shadow: var(--shadow-md);
  transform-origin: top center;
  /* initial hidden state */
  opacity: 0;
  transform: scaleY(0.92) translateY(-4px);
  pointer-events: none;
  transition: opacity 120ms var(--ease-in),
              transform 120ms var(--ease-in);
}
.dropdown-menu.open {
  opacity: 1;
  transform: scaleY(1) translateY(0);
  pointer-events: auto;
  transition: opacity 200ms var(--ease-out),
              transform 200ms var(--ease-out);
}

/* Menu items stagger (optional, for ≤8 items) */
.dropdown-menu.open > * {
  animation: fadeInUp 0.25s var(--ease-out) both;
}
.dropdown-menu.open > *:nth-child(1) { animation-delay: 30ms; }
.dropdown-menu.open > *:nth-child(2) { animation-delay: 50ms; }
.dropdown-menu.open > *:nth-child(3) { animation-delay: 70ms; }
.dropdown-menu.open > *:nth-child(4) { animation-delay: 90ms; }
.dropdown-menu.open > *:nth-child(5) { animation-delay: 110ms; }
```

**React + Framer Motion 方案:**
```tsx
import { motion, AnimatePresence } from 'framer-motion'

function DropdownMenu({ isOpen, children, align = 'left' }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          initial={{ opacity: 0, scaleY: 0.92, y: -4 }}
          animate={{ opacity: 1, scaleY: 1, y: 0 }}
          exit={{ opacity: 0, scaleY: 0.92, y: -4 }}
          transition={{
            duration: 0.2,
            exit: { duration: 0.12 },
            ease: [0.16, 1, 0.3, 1],
          }}
          style={{
            position: 'absolute',
            top: '100%',
            [align]: 0,
            marginTop: 4,
            zIndex: 50,
            background: 'var(--white)',
            border: '1px solid var(--stroke)',
            borderRadius: 0,
            boxShadow: 'var(--shadow-md)',
            transformOrigin: 'top center',
            overflow: 'hidden',
          }}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}
```

### Popover / Date Picker

与 Dropdown 类似，但从触发元素中心方向展开。Shadow: `--shadow-md`。

**React + Framer Motion 方案:**
```tsx
function Popover({ isOpen, children, position = 'bottom' }) {
  const origins = {
    top: 'bottom center',
    bottom: 'top center',
    left: 'center right',
    right: 'center left',
  }
  const offsets = {
    top: { y: 4 },
    bottom: { y: -4 },
    left: { x: 4 },
    right: { x: -4 },
  }

  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          initial={{ opacity: 0, scale: 0.95, ...offsets[position] }}
          animate={{ opacity: 1, scale: 1, x: 0, y: 0 }}
          exit={{ opacity: 0, scale: 0.95, ...offsets[position] }}
          transition={{
            duration: 0.2,
            exit: { duration: 0.12 },
            ease: [0.16, 1, 0.3, 1],
          }}
          style={{
            position: 'absolute',
            zIndex: 50,
            background: 'var(--white)',
            border: '1px solid var(--stroke)',
            borderRadius: 0,
            boxShadow: 'var(--shadow-md)',
            transformOrigin: origins[position],
          }}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}
```

**CSS 方案 (bottom position):**
```css
.popover {
  position: absolute;
  z-index: 50;
  background: var(--white);
  border: 1px solid var(--stroke);
  border-radius: 0;
  box-shadow: var(--shadow-md);
  transform-origin: top center;
  opacity: 0;
  transform: scale(0.95) translateY(-4px);
  pointer-events: none;
  transition: opacity 120ms var(--ease-in),
              transform 120ms var(--ease-in);
}
.popover.open {
  opacity: 1;
  transform: scale(1) translateY(0);
  pointer-events: auto;
  transition: opacity 200ms var(--ease-out),
              transform 200ms var(--ease-out);
}
```

### Modal / Dialog

居中浮层，带半透明遮罩。Shadow: `--shadow-lg`。遮罩和内容分开编排，遮罩先入后出。

**React + Framer Motion 方案:**
```tsx
function Modal({ isOpen, onClose, children }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <div className="fixed inset-0 z-[100] flex items-center justify-center">
          {/* Backdrop */}
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.2 }}
            onClick={onClose}
            style={{
              position: 'absolute',
              inset: 0,
              background: 'rgba(0, 0, 0, 0.3)',
            }}
          />
          {/* Content */}
          <motion.div
            initial={{ opacity: 0, scale: 0.95, y: 12 }}
            animate={{ opacity: 1, scale: 1, y: 0 }}
            exit={{ opacity: 0, scale: 0.95, y: 12 }}
            transition={{
              duration: 0.25,
              exit: { duration: 0.15 },
              ease: [0.16, 1, 0.3, 1],
            }}
            style={{
              position: 'relative',
              background: 'var(--white)',
              borderRadius: 0,
              boxShadow: 'var(--shadow-lg)',
              padding: 'var(--space-xl)',
              width: '90vw',
              maxWidth: 560,
              maxHeight: '85vh',
              overflow: 'auto',
            }}
          >
            {children}
          </motion.div>
        </div>
      )}
    </AnimatePresence>
  )
}
```

**CSS 方案:**
```css
/* Backdrop */
.modal-backdrop {
  position: fixed;
  inset: 0;
  z-index: 100;
  background: rgba(0, 0, 0, 0.3);
  opacity: 0;
  transition: opacity 150ms var(--ease-in);
  pointer-events: none;
}
.modal-backdrop.open {
  opacity: 1;
  pointer-events: auto;
  transition: opacity 200ms var(--ease-out);
}

/* Content */
.modal-content {
  position: fixed;
  z-index: 101;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0.95) translateY(12px);
  background: var(--white);
  border-radius: 0;
  box-shadow: var(--shadow-lg);
  padding: var(--space-xl);
  width: 90vw;
  max-width: 560px;
  max-height: 85vh;
  overflow: auto;
  opacity: 0;
  transition: opacity 150ms var(--ease-in),
              transform 150ms var(--ease-in);
}
.modal-content.open {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1) translateY(0);
  transition: opacity 250ms var(--ease-out),
              transform 250ms var(--ease-out);
}
```

### Side Drawer

从右侧滑入的全高面板。Shadow: `--shadow-overlay`。

**React + Framer Motion 方案:**
```tsx
function SideDrawer({ isOpen, onClose, children, width = 400 }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <div className="fixed inset-0 z-[100]">
          {/* Backdrop */}
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.25 }}
            onClick={onClose}
            style={{
              position: 'absolute',
              inset: 0,
              background: 'rgba(0, 0, 0, 0.3)',
            }}
          />
          {/* Drawer */}
          <motion.aside
            initial={{ x: '100%' }}
            animate={{ x: 0 }}
            exit={{ x: '100%' }}
            transition={{
              duration: 0.3,
              exit: { duration: 0.2 },
              ease: [0.16, 1, 0.3, 1],
            }}
            style={{
              position: 'absolute',
              top: 0,
              right: 0,
              bottom: 0,
              width,
              maxWidth: '90vw',
              background: 'var(--white)',
              borderRadius: 0,
              boxShadow: 'var(--shadow-overlay)',
              padding: 'var(--space-xl)',
              overflow: 'auto',
            }}
          >
            {children}
          </motion.aside>
        </div>
      )}
    </AnimatePresence>
  )
}
```

**CSS 方案:**
```css
.drawer-panel {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  z-index: 101;
  width: 400px;
  max-width: 90vw;
  background: var(--white);
  border-radius: 0;
  box-shadow: var(--shadow-overlay);
  padding: var(--space-xl);
  overflow: auto;
  transform: translateX(100%);
  transition: transform 200ms var(--ease-in);
}
.drawer-panel.open {
  transform: translateX(0);
  transition: transform 300ms var(--ease-out);
}
```

### 浮动层动效速查表

| 组件 | Shadow | 进入 | 退出 | Duration (in/out) |
|---|---|---|---|---|
| Dropdown Menu | `--shadow-md` | scaleY 0.92→1, y -4→0, fade in | 反向 | 200ms / 120ms |
| Popover | `--shadow-md` | scale 0.95→1, offset→0, fade in | 反向 | 200ms / 120ms |
| Modal | `--shadow-lg` | scale 0.95→1, y 12→0, fade in + backdrop | 反向 | 250ms / 150ms |
| Side Drawer | `--shadow-overlay` | x 100%→0 + backdrop | 反向 | 300ms / 200ms |
| Toast | `--shadow-md` | y 20→0, fade in | x→100, fade out | spring / spring |

---

## 7. FORM INTERACTIONS

**Input Focus — Border Highlight (V3: sharp, minimal):**
```css
.lanbow-input {
  font-family: var(--font-ui);
  font-size: 14px;
  color: var(--grey-01);
  background: var(--white);
  border: 1px solid var(--grey-12);
  border-radius: 0;
  padding: 10px 12px;
  width: 100%;
  outline: none;
  transition: border-color 0.15s var(--ease-out);
}
.lanbow-input:focus {
  border-color: var(--grey-01);
}
.lanbow-input::placeholder {
  color: var(--grey-08);
}
```

**Shake on Error:**
```tsx
import { motion, useAnimation } from 'framer-motion'

function ShakeInput({ error, ...props }) {
  const controls = useAnimation();
  useEffect(() => {
    if (error) {
      controls.start({
        x: [0, -8, 8, -8, 8, 0],
        transition: { duration: 0.4 },
      });
    }
  }, [error, controls]);

  return (
    <motion.div animate={controls}>
      <input
        {...props}
        className="lanbow-input"
        style={{ borderColor: error ? 'var(--red)' : undefined }}
      />
      {error && (
        <motion.p
          initial={{ opacity: 0, y: -8 }}
          animate={{ opacity: 1, y: 0 }}
          style={{
            fontFamily: 'var(--font-ui)', fontSize: 12,
            color: 'var(--red)', marginTop: 4,
          }}
        >
          {error}
        </motion.p>
      )}
    </motion.div>
  );
}
```

---

## 8. KPI DATA — NUMBER COUNT-UP

```tsx
import { motion, useMotionValue, useTransform, animate } from 'framer-motion'
import { useEffect } from 'react'

function CountUp({ value, duration = 1.5 }) {
  const count = useMotionValue(0);
  const rounded = useTransform(count, (v) =>
    v.toLocaleString('en-US', { maximumFractionDigits: 0 })
  );

  useEffect(() => {
    const controls = animate(count, value, {
      duration,
      ease: [0.16, 1, 0.3, 1],
    });
    return controls.stop;
  }, [value]);

  return (
    <motion.span className="kpi-value">{rounded}</motion.span>
  );
}

// Usage: <CountUp value={248900} />
```

---

## 9. PAGE TRANSITIONS

```tsx
import { AnimatePresence, motion } from 'framer-motion'

const pageVariants = {
  initial: { opacity: 0, y: 12 },
  enter: { opacity: 1, y: 0 },
  exit: { opacity: 0, y: -12 },
};

function LanbowPageTransition({ children, pageKey }) {
  return (
    <AnimatePresence mode="wait">
      <motion.div
        key={pageKey}
        variants={pageVariants}
        initial="initial"
        animate="enter"
        exit="exit"
        transition={{ duration: 0.3, ease: [0.16, 1, 0.3, 1] }}
      >
        {children}
      </motion.div>
    </AnimatePresence>
  );
}
```

---

## 10. SKELETON LOADING (V3 Style)

```tsx
function CardSkeleton() {
  return (
    <div className="card" style={{ padding: 'var(--space-xl)' }}>
      <div className="animate-pulse" style={{ display: 'flex', flexDirection: 'column', gap: 12 }}>
        <div style={{ height: 28, width: '40%', background: 'var(--grey-12)' }} />
        <div style={{ height: 40, width: '60%', background: 'var(--grey-12)' }} />
        <div style={{ height: 160, width: '100%', background: 'var(--card-warm-grey)' }} />
        <div style={{ display: 'flex', gap: 16 }}>
          <div style={{ height: 12, width: 60, background: 'var(--grey-12)' }} />
          <div style={{ height: 12, width: 60, background: 'var(--grey-12)' }} />
        </div>
      </div>
    </div>
  );
}
```

## CSS KEYFRAME ANIMATIONS

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

## PERFORMANCE BEST PRACTICES

1. **只动 `transform` 和 `opacity`** — 保证 60fps，避免动 `width/height/top/left`
2. **谨慎使用 `will-change`** — 仅在确实需要 GPU 加速的元素上
3. **Spring > Linear** — 使用弹性动画更自然，匹配 Lanbow 的品质感
4. **可中断** — 允许用户在动画过程中操作，不要阻塞交互
5. **渐进增强** — 无 JS 时也能正常使用，动效是增强而非必需
