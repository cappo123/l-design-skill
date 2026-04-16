# Lanbow V3 — Motion Patterns (Implementation)

具体动效组件的 React + CSS 实现。基础规则见 `motion-tokens.md`。

---

## 1. BUTTON MICROINTERACTIONS

### Primary Button — Press Feedback

```tsx
import { motion } from 'framer-motion'

function LanbowButton({ children, onClick, variant = 'primary' }: {
  children: React.ReactNode
  onClick?: () => void
  variant?: 'primary' | 'secondary'
}) {
  return (
    <motion.button
      onClick={onClick}
      whileHover={{ opacity: 0.85 }}
      whileTap={{ scale: 0.97 }}
      transition={{ type: 'spring', stiffness: 400, damping: 17 }}
      className={variant === 'primary' ? 'btn-primary' : 'btn-secondary'}
    >
      {children}
    </motion.button>
  )
}
```

### Loading Button — State Transition

```tsx
import { motion, AnimatePresence } from 'framer-motion'

function LoadingButton({ isLoading, children, onClick }: {
  isLoading: boolean
  children: React.ReactNode
  onClick?: () => void
}) {
  return (
    <motion.button
      onClick={onClick}
      disabled={isLoading}
      whileTap={!isLoading ? { scale: 0.97 } : {}}
      className="btn-primary btn-loading"
    >
      <AnimatePresence mode="wait">
        {isLoading ? (
          <motion.span
            key="loading"
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -10 }}
            className="btn-loading-content"
          >
            <svg className="animate-spin" width="16" height="16" viewBox="0 0 24 24">
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

### "View All" Text Button — Underline Hover

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

### Card Hover — Subtle Lift

```css
.card-interactive {
  transition: transform 0.2s var(--ease-out);
  cursor: pointer;
}
.card-interactive:hover {
  transform: translateY(-2px);
}
```

### Report Sub-Card — Click Feedback (React)

```tsx
function ReportCardInteractive({ children, onClick }: {
  children: React.ReactNode
  onClick?: () => void
}) {
  return (
    <motion.div
      onClick={onClick}
      whileHover={{ y: -2 }}
      whileTap={{ scale: 0.985 }}
      transition={{ type: 'spring', stiffness: 400, damping: 25 }}
      className="card-warm card-interactive"
    >
      {children}
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

### Fade-In on Scroll (Intersection Observer)

```tsx
function useInView({ threshold = 0, triggerOnce = false } = {}) {
  const ref = useRef<HTMLDivElement>(null);
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

  return [ref, isInView] as const;
}

function FadeInSection({ children }: { children: React.ReactNode }) {
  const [ref, isInView] = useInView({ threshold: 0.2, triggerOnce: true });
  return (
    <div
      ref={ref}
      className={`fade-in-section ${isInView ? 'is-visible' : ''}`}
    >
      {children}
    </div>
  );
}
```

```css
.fade-in-section {
  opacity: 0;
  transform: translateY(32px);
  transition: opacity 0.7s var(--ease-out), transform 0.7s var(--ease-out);
}
.fade-in-section.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

### Scroll Progress Bar (Cyan accent)

```tsx
import { motion, useScroll, useSpring } from 'framer-motion'

function ScrollProgress() {
  const { scrollYProgress } = useScroll();
  const scaleX = useSpring(scrollYProgress, {
    stiffness: 100, damping: 30, restDelta: 0.001,
  });
  return (
    <motion.div
      className="scroll-progress"
      style={{ scaleX, backgroundColor: 'var(--lanbow-cyan)' }}
    />
  );
}
```

```css
.scroll-progress {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  transform-origin: left;
  z-index: 50;
}
```

---

## 5. NAVIGATION — Active Link Indicator

```tsx
function LanbowNav({ items, activeItem }: {
  items: { id: string; href: string; label: string }[]
  activeItem: string
}) {
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
              className={`nav-link ${item.id === activeItem ? 'active' : ''}`}
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

## 6. FLOATING LAYERS

### Dropdown Menu

**CSS:**
```css
.dropdown-menu {
  position: absolute;
  z-index: 50;
  background: var(--white);
  border: 1px solid var(--stroke);
  border-radius: 0;
  box-shadow: var(--shadow-md);
  transform-origin: top center;
  opacity: 0;
  transform: scaleY(0.92) translateY(-4px);
  pointer-events: none;
  transition: opacity 120ms var(--ease-in), transform 120ms var(--ease-in);
}
.dropdown-menu.open {
  opacity: 1;
  transform: scaleY(1) translateY(0);
  pointer-events: auto;
  transition: opacity 200ms var(--ease-out), transform 200ms var(--ease-out);
}
```

**React + Framer Motion:**
```tsx
function DropdownMenu({ isOpen, children, align = 'left' }: {
  isOpen: boolean
  children: React.ReactNode
  align?: 'left' | 'right'
}) {
  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          className="dropdown-menu open"
          initial={{ opacity: 0, scaleY: 0.92, y: -4 }}
          animate={{ opacity: 1, scaleY: 1, y: 0 }}
          exit={{ opacity: 0, scaleY: 0.92, y: -4 }}
          transition={{ duration: 0.2, exit: { duration: 0.12 }, ease: [0.16, 1, 0.3, 1] }}
          style={{ [align]: 0, top: '100%', marginTop: 4 }}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}
```

### Popover / Date Picker

```tsx
function Popover({ isOpen, children, position = 'bottom' }: {
  isOpen: boolean
  children: React.ReactNode
  position?: 'top' | 'bottom' | 'left' | 'right'
}) {
  const origins: Record<string, string> = {
    top: 'bottom center', bottom: 'top center',
    left: 'center right', right: 'center left',
  }
  const offsets: Record<string, object> = {
    top: { y: 4 }, bottom: { y: -4 },
    left: { x: 4 }, right: { x: -4 },
  }

  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          className="popover open"
          initial={{ opacity: 0, scale: 0.95, ...offsets[position] }}
          animate={{ opacity: 1, scale: 1, x: 0, y: 0 }}
          exit={{ opacity: 0, scale: 0.95, ...offsets[position] }}
          transition={{ duration: 0.2, exit: { duration: 0.12 }, ease: [0.16, 1, 0.3, 1] }}
          style={{ transformOrigin: origins[position] }}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}
```

```css
.popover {
  position: absolute;
  z-index: 50;
  background: var(--white);
  border: 1px solid var(--stroke);
  border-radius: 0;
  box-shadow: var(--shadow-md);
}
```

### Modal / Dialog

```tsx
function Modal({ isOpen, onClose, children }: {
  isOpen: boolean
  onClose: () => void
  children: React.ReactNode
}) {
  return (
    <AnimatePresence>
      {isOpen && (
        <div className="modal-overlay">
          <motion.div
            className="modal-backdrop"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.2 }}
            onClick={onClose}
          />
          <motion.div
            className="modal-content"
            initial={{ opacity: 0, scale: 0.95, y: 12 }}
            animate={{ opacity: 1, scale: 1, y: 0 }}
            exit={{ opacity: 0, scale: 0.95, y: 12 }}
            transition={{ duration: 0.25, exit: { duration: 0.15 }, ease: [0.16, 1, 0.3, 1] }}
          >
            {children}
          </motion.div>
        </div>
      )}
    </AnimatePresence>
  )
}
```

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal-backdrop {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.3);
}
.modal-content {
  position: relative;
  background: var(--white);
  border-radius: 0;
  box-shadow: var(--shadow-lg);
  padding: var(--space-xl);
  width: 90vw;
  max-width: 560px;
  max-height: 85vh;
  overflow: auto;
}
```

### Side Drawer

```tsx
function SideDrawer({ isOpen, onClose, children, width = 400 }: {
  isOpen: boolean
  onClose: () => void
  children: React.ReactNode
  width?: number
}) {
  return (
    <AnimatePresence>
      {isOpen && (
        <div className="modal-overlay">
          <motion.div
            className="modal-backdrop"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.25 }}
            onClick={onClose}
          />
          <motion.aside
            className="drawer-panel"
            initial={{ x: '100%' }}
            animate={{ x: 0 }}
            exit={{ x: '100%' }}
            transition={{ duration: 0.3, exit: { duration: 0.2 }, ease: [0.16, 1, 0.3, 1] }}
            style={{ width, maxWidth: '90vw' }}
          >
            {children}
          </motion.aside>
        </div>
      )}
    </AnimatePresence>
  )
}
```

```css
.drawer-panel {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  z-index: 101;
  background: var(--white);
  border-radius: 0;
  box-shadow: var(--shadow-overlay);
  padding: var(--space-xl);
  overflow: auto;
}
```

### Toast Notification

```tsx
function ToastContainer({ toasts, onDismiss }: {
  toasts: { id: string; message: string }[]
  onDismiss: (id: string) => void
}) {
  return (
    <div className="toast-container">
      <AnimatePresence>
        {toasts.map((toast) => (
          <motion.div
            key={toast.id}
            className="toast"
            initial={{ opacity: 0, y: 20, scale: 0.95 }}
            animate={{ opacity: 1, y: 0, scale: 1 }}
            exit={{ opacity: 0, x: 100, scale: 0.95 }}
            transition={{ type: 'spring', stiffness: 400, damping: 25 }}
            onClick={() => onDismiss(toast.id)}
          >
            {toast.message}
          </motion.div>
        ))}
      </AnimatePresence>
    </div>
  );
}
```

```css
.toast-container {
  position: fixed;
  bottom: var(--space-xl);
  right: var(--space-xl);
  display: flex;
  flex-direction: column;
  gap: var(--space-sm);
  z-index: 50;
}
.toast {
  background: var(--grey-01);
  color: var(--white);
  font-family: var(--font-ui);
  font-size: 13px;
  padding: var(--space-md) var(--space-lg);
  border-radius: 0;
  cursor: pointer;
  min-width: 240px;
  box-shadow: var(--shadow-md);
}
```

---

## 7. FORM INTERACTIONS

### Input Focus — Border Highlight

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
.lanbow-input.error {
  border-color: var(--red);
}
```

### Shake on Error (React)

```tsx
import { motion, useAnimation } from 'framer-motion'

function ShakeInput({ error, ...props }: {
  error?: string
  [key: string]: any
}) {
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
      <input {...props} className={`lanbow-input ${error ? 'error' : ''}`} />
      {error && (
        <motion.p
          className="input-error-text"
          initial={{ opacity: 0, y: -8 }}
          animate={{ opacity: 1, y: 0 }}
        >
          {error}
        </motion.p>
      )}
    </motion.div>
  );
}
```

```css
.input-error-text {
  font-family: var(--font-ui);
  font-size: 12px;
  color: var(--red);
  margin-top: var(--space-xs);
}
```

---

## 8. KPI DATA — NUMBER COUNT-UP

```tsx
import { motion, useMotionValue, useTransform, animate } from 'framer-motion'

function CountUp({ value, duration = 1.5 }: {
  value: number
  duration?: number
}) {
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
  }, [value, count, duration]);

  return <motion.span className="kpi-value">{rounded}</motion.span>;
}
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

function LanbowPageTransition({ children, pageKey }: {
  children: React.ReactNode
  pageKey: string
}) {
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
    <div className="card">
      <div className="skeleton-group">
        <div className="skeleton-bar" style={{ height: 28, width: '40%' }} />
        <div className="skeleton-bar" style={{ height: 40, width: '60%' }} />
        <div className="skeleton-bar skeleton-bar--warm" style={{ height: 160, width: '100%' }} />
        <div className="skeleton-row">
          <div className="skeleton-bar" style={{ height: 12, width: 60 }} />
          <div className="skeleton-bar" style={{ height: 12, width: 60 }} />
        </div>
      </div>
    </div>
  );
}
```

```css
.skeleton-group {
  display: flex;
  flex-direction: column;
  gap: var(--space-md);
  animation: pulse 1.8s ease-in-out infinite;
}
.skeleton-bar {
  background: var(--grey-12);
}
.skeleton-bar--warm {
  background: var(--card-warm-grey);
}
.skeleton-row {
  display: flex;
  gap: var(--space-base);
}
```
