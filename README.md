# Thinking Review Expert

![npm version](https://badge.fury.io/js/thinking-review-expert.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

> **Automatic validation for 7-circle sacred thinking flows**

When you use thinking tools (Sequential, Tractatus, or Debug), this skill automatically validates your session for quality, completeness, and logical coherence. It applies stop-slop writing principles and generates professional quality scores.

---

## Installation

```bash
# Claude Code / Claude Skills
git clone https://github.com/Alot1z/thinking-review-expert.git
cp -r thinking-review-expert ~/.claude/skills/

# NPM
npm install -g thinking-review-expert
# or
npm install --save-dev thinking-review-expert
```

---

## What It Does

This skill automatically activates when you use thinking tools and provides:

| Feature | Description |
|---------|-------------|
| **7-Circle Validation** | Checks compliance across all sacred thinking circles |
| **Embedded Tools** | Sequential (203 lines), Tractatus (353 lines), Debug (388 lines) |
| **Quality Scoring** | 0-100 score with specific recommendations |
| **Stop-Slop** | Auto-applies direct, active language principles |
| **Token Savings** | 85-90% savings via lazy loading |

---

## How It Works

```
You use thinking tool
    ↓
Skill detects usage automatically
    ↓
Validates 7-circle compliance
    ↓
Applies stop-slop principles
    ↓
Generates quality report (0-100)
    ↓
If score < 80: Provides specific recommendations
```

---

## 7-Circle Validation

```
Circle 1 (Vision)    → Purpose & goals defined
Circle 2 (Research)   → Information gathered (T→S→D rotation)
Circle 3 (Structure)  → Architecture designed
Circle 4 (Design)     → Trade-offs analyzed (T→S→D rotation)
Circle 5 (Build)      → Implementation planned
Circle 6 (Validate)   → Quality gates passed (T→S→D rotation)
Circle 7 (Integrate)  → System unified
```

**Tool Rotation:**
- Circles 1,3,5,7: Sequential → Tractatus → Debug
- Circles 2,4,6: Tractatus → Sequential → Debug

---

## Quick Start

```typescript
// Sequential Thinking
sequentialthinking(
  thought="Analyze system bottlenecks",
  thoughtNumber=1,
  totalThoughts=5
)

// Tractatus Thinking
tractatusthinking(
  operation="start",
  concept="What is token optimization?"
)

// Debug Thinking
debug_thinking(
  action="create",
  nodeType="problem",
  content="Agent spawning fails"
)
```

The skill automatically detects these calls and validates your session.

---

## Stop-Slop Writing

**Before (AI Slop):**
> "I was wondering if perhaps you might want to consider potentially exploring the possibility of enhancing..."

**After (Stop-Slop):**
> "Add 2 more thought cycles. Include alternative perspectives."

**Principles Applied:**
- Direct language (no hedging)
- Active voice (no passive)
- Specific terms (no vague modifiers)
- No buzzwords (no corporate speak)

---

## Embedded Tools

| Tool | Lines | Features |
|------|-------|----------|
| Sequential Thinking | 203 | 6 specialized agents, adaptive routing |
| Tractatus Thinking | 353 | 8 operations, proposition trees |
| Debug Thinking | 388 | Graph tracking, 6 nodes, 8 edges |

**Total:** 944 lines of embedded implementations

---

## Configuration

Optional environment variables:

```bash
THINKING_REVIEW_AUTO_DIAGRAMS=true    # Enable diagrams (default: true)
THINKING_REVIEW_MERMAID_THEME=zinc-dark  # Diagram theme
THINKING_REVIEW_STOP_SLOP=true        # Apply stop-slop (default: true)
```

---

## Performance

| Operation | Tokens | Time |
|-----------|--------|------|
| Sequential (1 thought) | 300-1500 | ~1s |
| Tractatus (1 operation) | 200-1000 | ~1s |
| Debug (1 node) | 100-500 | ~1s |
| Full validation | 500-1500 | ~5s |

**Token Savings:** 85-90% with lazy loading

---

## Related Skills

| Skill | Description | Link |
|-------|-------------|------|
| sequential-thinking | Step-by-step reasoning | [npm](https://www.npmjs.com/package/sequential-thinking) |
| tractatus-thinking | Logical analysis | [npm](https://www.npmjs.com/package/tractatus-thinking) |
| debug-thinking | Problem tracking | [npm](https://www.npmjs.com/package/debug-thinking) |
| code-review-expert | Code validation | [npm](https://www.npmjs.com/package/code-review-expert) |
| deslop | Remove AI slop | [npm](https://www.npmjs.com/package/deslop) |

---

## Links

- **GitHub:** https://github.com/Alot1z/thinking-review-expert
- **NPM:** https://www.npmjs.com/package/thinking-review-expert
- **License:** MIT

---

**Version:** 6.0.1  
**Author:** Alot1z
