# Stop-Slop Writing Principles

**Source**: https://github.com/hardikpandya/stop-slop  
**Embedded**: v1.0.0  
**Purpose**: Remove AI-generated code slop, ensure clear direct writing

---

## Core Principle

**Simple is better than complex.**

Avoid:
- ❌ Buzzwords ("paradigm-shifting", "synergistic", "leverage")
- ❌ Redundant phrases ("at this point in time")
- ❌ Corporate jargon ("drill down", "circle back")
- ❌ Passive voice ("mistakes were made")

Use:
- ✅ Active voice ("I found an issue")
- ✅ Specific language ("add 2 thoughts" not "enhance depth")
- ✅ Clear directives ("do this" not "consider doing this")
- ✅ Honest assessments ("this is wrong" not "this presents an opportunity")

---

## Common AI Slop Patterns

### Over-Commenting

**❌ AI SLOP:**
```javascript
// Function to add two numbers
// Takes two parameters a and b
// Returns the sum of a and b
function add(a, b) {
  // Return the result of adding a and b
  return a + b;
}
```

**✅ CLEAN:**
```javascript
function add(a, b) {
  return a + b;
}
```

### Unnecessary Abstractions

**❌ AI SLOP:**
```javascript
class NumberAdder {
  constructor() { this.numbers = []; }
  addNumber(number) { this.numbers.push(number); }
  getSum() {
    return this.numbers.reduce((a, b) => a + b, 0);
  }
}
```

**✅ CLEAN:**
```javascript
function sum(numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```

### Verbose Error Handling

**❌ AI SLOP:**
```javascript
async function fetchData(url) {
  try {
    const response = await fetch(url);
    if (response.ok) {
      const data = await response.json();
      return { success: true, data: data };
    } else {
      return { success: false, error: 'Request failed' };
    }
  } catch (error) {
    return { success: false, error: error.message };
  }
}
```

**✅ CLEAN:**
```javascript
async function fetchData(url) {
  const response = await fetch(url);
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  return response.json();
}
```

### Redundant Validations

**❌ AI SLOP:**
```javascript
function processString(str) {
  if (typeof str === 'string') {
    if (str.length > 0) {
      if (str.trim().length > 0) {
        return str.toUpperCase();
      }
    }
  }
  return '';
}
```

**✅ CLEAN:**
```javascript
function processString(str) {
  return str?.trim() ? str.trim().toUpperCase() : '';
}
```

---

## Slop Indicators

```yaml
slop_indicators:
  over_commenting:
    - Comments stating the obvious
    - More comments than code
    - Commented-out code left in
  
  unnecessary_complexity:
    - Classes for simple functions
    - Multiple abstraction layers
    - Design patterns overuse
  
  verbosity:
    - Long variable names
    - Redundant conditions
    - Unnecessary intermediate variables
  
  poor_practices:
    - Missing error handling
    - Inefficient loops
    - Console.logs left in
    - Magic numbers
```

---

## Refactoring Principles

```yaml
principles:
  simplicity:
    - Use simple solutions
    - Avoid over-engineering
    - Prefer built-in methods
    - Minimize abstractions
    
  clarity:
    - Self-documenting code
    - Meaningful names
    - Logical structure
    - Minimal comments
    
  efficiency:
    - Optimize algorithms
    - Reduce iterations
    - Use appropriate data structures
    - Avoid redundant operations
    
  quality:
    - Proper error handling
    - Edge cases covered
    - Tests passing
    - Consistent style
```

---

## Writing Checklist

Before finalizing any text, ask:

- [ ] Am I using buzzwords? Replace with specific terms
- [ ] Am I using passive voice? Change to active
- [ ] Am I over-explaining? Cut to essentials
- [ ] Am I being indirect? Be direct
- [ ] Am I using corporate jargon? Use plain language
- [ ] Is this self-documenting? Remove if obvious

---

## Examples

### Before (AI Slop)
```
In order to facilitate the enhancement of the cognitive 
processing capabilities, it is recommended that 
practitioners leverage the multi-agent coordination paradigm.
```

### After (Clean)
```
Use multi-agent coordination to improve cognitive processing.
```

### Before (AI Slop)
```
The system facilitates the optimization of token usage 
through the implementation of sophisticated caching 
mechanisms and lazy loading strategies.
```

### After (Clean)
```
Optimize tokens with caching and lazy loading.
```

---

**Embedded From**: `deslop` skill  
**Version**: 1.0.0  
**License**: MIT
