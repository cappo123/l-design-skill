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
