# Embedded Tools Reference

**Lazy Loading** - Each file loads on demand (~200-400 tokens each)

---

## Quick Reference

| Tool | What It Does | When to Use | Lines | Load |
|------|--------------|-------------|-------|-----|
| **Sequential Thinking** | Multi-agent reasoning (6 agents) | Multi-step problems, process planning | 203 | on demand |
| **Tractatus Thinking** | Logical concept analysis (8 operations) | Concept analysis, definitions, structure | 353 | on demand |
| **Debug Thinking** | Problem tracking (6 nodes, 8 edges) | Bug investigation, learning from issues | 388 | on demand |
| **Context7** | Documentation retrieval | Latest docs, API references | 300 | on demand |
| **DeepWiki** | GitHub repository research | Best practices, patterns from repos | 406 | on demand |

**Total:** 1,650 lines of embedded implementations

---

## How to Use

### Load Tool Documentation

```markdown
# Sequential Thinking

Load: `tools/embedded-sequential-thinking.md`

## When to Use
- Multi-step problem solving
- Process planning and execution
- Complex decision making
- Agent-coordinated analysis

## Quick Example
```
Thought 1/7: "Analyze system bottlenecks"
→ [Factual] Need metrics
→ [Creative] Could be database, network, or code
→ [Synthesis] Check database first
```

[See full documentation...]
```

### Tool Priority (Embedded-First)

```python
# ALWAYS prioritize embedded tools (v5.0)
def use_thinking_tool(tool_name, params):
    if has_embedded_implementation(tool_name):
        return use_embedded_tool(tool_name, params)  # FIRST
    if mcp_server_available(tool_name):
        return call_mcp_tool(tool_name, params)  # FALLBACK
    raise ToolNotAvailableError(tool_name)
```

---

## Tool Summaries

### Sequential Thinking (embedded-sequential-thinking.md)

**6 Specialized Agents:**
1. **Factual** - Data verification, fact-checking
2. **Emotional** - User impact, sentiment analysis
3. **Critical** - Logic validation, flaw detection
4. **Optimistic** - Best-case scenarios
5. **Creative** - Alternative solutions
6. **Synthesis** - Combines all perspectives

**Agent Routing:**
```python
if thought_number <= 3:
    return ['factual', 'creative']
elif thought_number <= total_thoughts * 0.7:
    return ['critical', 'emotional']
elif thought_number < total_thoughts:
    return ['optimistic']
else:
    return ['synthesis']
```

**Parameters:** `thought`, `thoughtNumber`, `totalThoughts`, `nextThoughtNeeded`, `isRevision`, `branchFromThought`, `branchId`, `needsMoreThoughts`

### Tractatus Thinking (embedded-tractatus-thinking.md)

**8 Operations:**
1. **start** - Begin concept analysis
2. **add** - Add proposition to tree
3. **navigate** - Explore proposition tree
4. **export** - Save structure (markdown/json/graphviz)
5. **analyze** - Check completeness
6. **revise** - Refine proposition
7. **undo** - Remove proposition
8. **move** - Restructure tree

**5 Decomposition Types:**
- `clarification` - Define terms, remove ambiguity
- `analysis` - Break into components
- `cases` - Enumerate possibilities (if/then)
- `implication` - What follows from this?
- `negation` - What would make this false?

**Session Structure:**
```python
tractatus_sessions = {
    'session_id': {
        'concept': str,
        'propositions': {
            '1': {'content': str, 'children': ['1.1', '1.2'], 'depth': 0}
        },
        'max_depth': 5,
        'style': 'analytical'
    }
}
```

### Debug Thinking (embedded-debug-thinking.md)

**6 Node Types:**
- `problem` - Error or bug to solve
- `hypothesis` - Proposed cause
- `experiment` - Test to validate
- `observation` - Result from test
- `learning` - Insight gained
- `solution` - Verified fix

**8 Edge Types:**
| Type | Direction | Meaning |
|------|-----------|---------|
| `decomposes` | problem → problem | Breaks into sub-problem |
| `hypothesizes` | problem → hypothesis | Proposes cause |
| `tests` | hypothesis → experiment | Validates |
| `produces` | experiment → observation | Yields result |
| `learns` | observation → learning | Extracts insight |
| `contradicts` | evidence → hypothesis | Evidence against |
| `supports` | evidence → hypothesis | Evidence for |
| `solves` | solution → problem | Resolves |

**Query Types:**
- `similar-problems` - Find past issues (with similarity scoring)
- `recent-activity` - Recent debugging sessions

### Context7 (embedded-context7.md)

**Purpose:** Fetch up-to-date documentation for any library

**2 Operations:**
1. **resolve-library-id** - Convert package name to Context7 ID
2. **get-library-docs** - Fetch documentation by ID

**Common Library Cache:**
```javascript
// 40+ common libraries pre-cached
'react' → '/facebook/react'
'next.js' → '/vercel/next.js'
'prisma' → '/prisma/prisma'
'tailwind' → '/tailwindlabs/tailwindcss'
```

**Modes:**
- `code` - API references, code examples
- `info` - Conceptual guides, architecture

**Example:**
```typescript
// Resolve
request = { libraryName: "next.js" }
response = { library_id: "/vercel/next.js" }

// Fetch docs
request = {
  context7CompatibleLibraryID: "/vercel/next.js",
  topic: "routing",
  mode: "code"
}
response = { content: "# Routing\n\n..." }
```

### DeepWiki (embedded-deepwiki.md)

**Purpose:** Research GitHub repositories with AI-powered analysis

**3 Operations:**
1. **ask_question** - Ask any question about a repo
2. **read_wiki_structure** - Get documentation topics
3. **read_wiki_contents** - View full documentation

**Common Repository Cache:**
```javascript
// 20+ common repos pre-cached
'facebook/react' → { topics: ['components', 'hooks', 'state'] }
'vercel/next.js' → { topics: ['routing', 'ssr', 'api-routes'] }
'reduxjs/redux' → { topics: ['actions', 'reducers', 'middleware'] }
```

**Example:**
```typescript
// Ask question
request = {
  repoName: "facebook/react",
  question: "What are hooks best practices?"
}
response = {
  answer: "Based on React repository...",
  sources: ["docs/hooks-rules.md"]
}

// Get structure
request = { repoName: "vercel/next.js" }
response = {
  topics: ["App Router", "Routing", "Rendering"]
}
```

---

## Integration with 7-Circle Flow

### Tool Rotation (T→S→D)

**Circles 1,3,5,7:** Sequential → Tractatus → Debug  
**Circles 2,4,6:** Tractatus → Sequential → Debug

### Enhancement Integration

**Context7 in Circle 2 (Research):**
```typescript
// Research latest approaches
context7_get_library_docs("/vercel/next.js", topic="routing")
// → Returns latest routing patterns
```

**DeepWiki in Circle 4 (Design):**
```typescript
// Study best practices
deepwiki_ask("facebook/react", "component patterns")
// → Returns community best practices
```

**Stop-Slop (Auto-applied):**
All outputs follow stop-slop principles:
- Direct language
- Active voice
- Specific terms
- No buzzwords

**Beautiful-Mermaid (Auto-applied):**
All diagrams follow beautiful-mermaid standards:
- 15 built-in themes
- Color system
- Typography standards
- Spacing constants

---

## Performance

### Token Efficiency

| Tool | Embedded | MCP | Savings |
|------|----------|-----|---------|
| Sequential | 300-1500 | 500-2000 | **40-50%** |
| Tractatus | 200-1000 | 300-1500 | **40-50%** |
| Debug | 100-500 | 200-800 | **40-50%** |
| Context7 | 150-800 | 300-1200 | **40-50%** |
| DeepWiki | 200-900 | 400-1500 | **40-50%** |

### Lazy Loading Benefits

**Without Lazy Loading:**
- Load all tools: ~2,000 tokens upfront
- All reference docs: ~1,000 tokens upfront
- **Total: ~3,000 tokens** (even if not using all tools)

**With Lazy Loading:**
- Root index: ~100 tokens
- Load tool when needed: ~200-400 tokens
- Load reference when needed: ~100-250 tokens
- **Total: ~300-750 tokens** (only what you use)

**Savings: 85-90%**

---

## Best Practices

1. **Start with Tractatus** - Understand WHAT before HOW
2. **Use Sequential for execution** - Step-by-step processing
3. **Use Debug for complexity** - Track multi-variable problems
4. **Use Context7 for docs** - Latest documentation
5. **Use DeepWiki for research** - GitHub best practices
6. **Apply Stop-Slop** - Direct, clear output
7. **Use Beautiful-Mermaid** - Professional diagrams

---

**Load on Demand** - Only load what you need (85-90% token savings)  
**Priority** - ALWAYS use embedded tools over MCP  
**Integration** - All tools work together in 7-circle flow
