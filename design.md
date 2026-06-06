# POWER-UP: Technical Design System & UI Architecture

## 1. Aesthetic Philosophy & Visual Identity
POWER-UP utilizes a "Premium Cyber-Physical" aesthetic. The design bridges the gap between high-end industrial design (hardware) and digital fluidity (software). It relies heavily on absolute contrast, deliberate negative space, and dynamic glassmorphism to establish a sense of weight and value.

The core visual anchor is the "bolt" glitch, representing energy transfer and effort-based reward.

### 1.1 CSS Custom Properties (Tokens)
The entire application is themed via a central set of CSS variables exposed on `:root` to ensure instantaneous UI updates and consistent theming.

**Color Palette:**
- `--background: #050508;` (Absolute Void)
- `--surface: rgba(17, 17, 24, 0.75);` (Translucent Depth)
- `--foreground: #f4f4f7;` (Crisp Off-White)
- `--muted: #8a8a9d;` (Desaturated Slate)
- `--graphite: #0c0c12;` (Deep Structural Element)
- `--accent: #f04a24;` (Kinetic Orange / High-Energy)
- `--accent-glow: rgba(240, 74, 36, 0.35);` (Radial Diffusion)
- `--accent-soft: rgba(240, 74, 36, 0.08);` (Interactive Surface)
- `--border-strong: rgba(255, 255, 255, 0.08);` (Structural Line)
- `--border-glow: rgba(240, 74, 36, 0.15);` (Active Border)

**Typography Stack:**
- `--font-sans: 'Outfit', 'Archivo', sans-serif;` (Geometric, wide, modern tracking)
- `--font-mono: 'Share Tech Mono', monospace;` (Technical readouts and scoring)

**Effects & Depth:**
- `--shadow-glow: 0 0 25px var(--accent-glow);` (Hover States / Active Elements)
- `--glass-bg: rgba(10, 10, 15, 0.65);` (Base component background)
- `--glass-border: rgba(255, 255, 255, 0.05);` (Subtle separation)
- `--glass-blur: blur(20px);` (Heavy backdrop filtering for volumetric UI)

---

## 2. Structural Architecture
The application is structured into a single `<div className="shell">` component that acts as the absolute container. 

### 2.1 Ambient Lighting
The `shell` implements pseudo-elements (`::before`, `::after`) generating `radial-gradient` spheres with severe `blur(40px)`. These spheres are fixed to opposite corners, creating an atmospheric background that shines through the `backdrop-filter: blur(20px)` applied to foreground components. This establishes the "Glassmorphism" effect.

### 2.2 Fluid Stage Routing
Instead of traditional page reloads, the application uses React State to dictate the visible "Stage" (`splash` -> `capture` -> `otp` -> `hub`).
- **Splash (`screen-splash`)**: An ephemeral, 800ms auto-advancing entry frame containing the logo and primary CTA. Designed to mask network latency and Firebase SDK initialization.
- **Auth Capture (`screen-auth`)**: A centralized identity capture card.
- **OTP (`screen-auth`)**: A state-flip of the auth card that animates in the verification input.
- **Hub (`screen-hub`)**: A CSS Grid architecture splitting into `hub-sidebar` (navigation/stats) and `hub-main` (the game canvas).

---

## 3. Micro-Interactions & Physics
Animations bypass generic easings (e.g., `ease-in-out`) in favor of custom cubic-beziers to simulate hardware physics and tension.

- **Spring Dynamics:** `transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);`
- **Button Hover States:** Buttons scale up fractionally (`transform: scale(1.02) translateY(-1px)`) and project a heavy `--shadow-glow`. The active state compresses (`transform: scale(0.98)`).
- **Error States:** An explicit `@keyframes shake` animation (`translateX` shifting 4px to -4px) provides immediate tactile feedback for invalid OTPs.

---

## 4. Game Integration & Canvas Abstraction
The `GameCanvas` component acts as a bridge between the DOM and hardware-accelerated 2D Canvas rendering.

### 4.1 Implementation Logic
Games (Snake, 2048, Tetris, Flappy) are implemented as isolated TypeScript classes handling their own `requestAnimationFrame` loops and rendering pipelines. 
- React explicitly ignores re-rendering the canvas via a `useRef` attachment.
- The React shell passes generic callbacks (`onEnd`, `onScore`) into the class constructors.

### 4.2 Effort-Based Scoring (The "Mind Rot" Loop)
The UX strictly enforces effort over time. Points are not passively accrued; they require explicit interactions (lines cleared, cells eaten, pipes passed).
When a game terminates, the `onEnd` callback triggers `buildResult(activeGame, score)` which algorithmically calculates standard points into "Power-Up Points" based on difficulty thresholds, assigning semantic labels (e.g., "Abysmal effort", "Strong effort") to drive behavioral reinforcement.

---

## 5. Security & Authentication UX
The Auth flow abstracts Firebase complexity into a seamless, Apple-like interface.
1. **Dynamic Identification**: The input field uses regex to determine if the payload is an email or an international phone number.
2. **Invisible reCAPTCHA**: `RecaptchaVerifier` is mounted to a hidden DOM node `id="recaptcha-container"`, silently proving human interaction without degrading the premium UI.
3. **Passwordless Priority**: Reduces friction entirely. The UI relies strictly on OTP codes or Email Magic Links (`sendSignInLinkToEmail`).
4. **State Restoration**: `onAuthStateChanged` listens passively. If a session token exists in indexedDB, the user skips the entire splash/auth flow and lands directly in the `hub`.

---

## 6. Layout Behaviors (Responsiveness)
The Hub utilizes CSS Grid to adapt flawlessly from Desktop to Mobile.
- **Desktop (`> 900px`)**: `grid-template-columns: 280px 1fr;`. The sidebar is fixed, and the game area occupies the remaining fluid space.
- **Mobile (`<= 900px`)**: The grid collapses into `flex-direction: column`. The sidebar becomes a horizontally scrollable strip or stacks below the game canvas depending on orientation to maximize the touch target area for gaming.

### 6.1 Safe Areas
Variables like `padding: calc(20px + env(safe-area-inset-bottom))` are explicitly defined for iOS devices to ensure the UI respects the Home Indicator and dynamic island physical notches.
