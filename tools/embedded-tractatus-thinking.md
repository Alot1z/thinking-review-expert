# Embedded Tractatus Thinking Tool

**Source**: https://github.com/Alot1z/tractatus-thinking
**License**: MIT
**Embedded**: v4.0.6

This tool provides logical concept analysis through structured decomposition WITHOUT requiring the MCP server.

## How It Works (Embedded)

Instead of calling an external MCP server, this skill uses Claude's native capabilities to perform Tractatus-style logical analysis:

1. **Concept Decomposition**: Break down concepts into atomic propositions
2. **Logical Structure**: Build hierarchical proposition trees
3. **Relationship Mapping**: Identify dependencies between propositions
4. **Atomic Detection**: Mark propositions that cannot decompose further

## Tool Operations

### 1. START - Initialize Analysis Session

```yaml
operation: start
concept:
  type: string
  required: true
  description: "The concept or question to analyze (e.g., 'What is token optimization?')"

thoughts:
  type: string
  required: false
  description: "Optional: Your raw thoughts for instant analysis (quick mode)"

depth_limit:
  type: integer
  default: 5
  minimum: 1
  maximum: 10
  description: "Maximum depth of decomposition"

style:
  type: string
  enum: [analytical, exhaustive, creative]
  default: analytical
  description: "Analysis style"

confidence_threshold:
  type: number
  default: 0.3
  minimum: 0.1
  maximum: 1.0
  description: "Minimum confidence for propositions"
```

**Embedded Implementation**:

```python
def start_analysis(concept, depth_limit=5, style='analytical'):
    """
    Initialize a Tractatus analysis session.
    
    Returns a session_id and initial proposition tree.
    """
    session_id = generate_uuid()
    
    # Root proposition is the concept itself
    root_proposition = {
        'number': '1',
        'content': concept,
        'depth': 0,
        'confidence': 0.5,
        'is_atomic': False
    }
    
    # Begin decomposition based on style
    if style == 'analytical':
        decompose_analytical(root_proposition, depth_limit)
    elif style == 'exhaustive':
        decompose_exhaustive(root_proposition, depth_limit)
    elif style == 'creative':
        decompose_creative(root_proposition, depth_limit)
    
    return {
        'session_id': session_id,
        'root': root_proposition,
        'propositions': build_proposition_tree()
    }
```

### 2. ADD - Build Understanding

```yaml
operation: add
session_id:
  type: string
  required: true
  description: "The session ID from start operation"

content:
  type: string
  required: true
  description: "The proposition content to add"

parent_number:
  type: string
  required: false
  default: null
  description: "Parent proposition number (null for root level)"

decomposition_type:
  type: string
  enum: [clarification, analysis, cases, implication, negation]
  description: "Type of logical decomposition"

is_atomic:
  type: boolean
  default: false
  description: "Whether proposition cannot be decomposed further"

confidence:
  type: number
  minimum: 0.1
  maximum: 1.0
  description: "Confidence level in this proposition"
```

**Decomposition Types**:

- **clarification**: Define terms, remove ambiguity
- **analysis**: Break into components
- **cases**: Enumerate possibilities (if/then scenarios)
- **implication**: What follows from this?
- **negation**: What would make this false?

### 3. NAVIGATE - Explore Relationships

```yaml
operation: navigate
session_id:
  type: string
  required: true

target:
  type: string
  required: true
  description: "Navigation target: proposition number, 'parent', 'child', 'sibling', or 'root'"

child_index:
  type: integer
  minimum: 0
  required: false
  description: "Index of child to navigate to (when target is 'child')"
```

### 4. EXPORT - Capture Insights

```yaml
operation: export
session_id:
  type: string
  required: true

format:
  type: string
  enum: [markdown, json, graphviz]
  default: markdown
  description: "Export format"

include_metadata:
  type: boolean
  default: false
  description: "Include analysis metadata in export"
```

**Markdown Export Format**:

```markdown
# Logical Analysis: {concept}

## 1. {root_proposition}
### 1.1. {child_proposition}
### 1.2. {child_proposition}
#### 1.2.1. {grandchild}
## 2. {second_root_proposition}

---

**Analysis Depth**: {max_depth}
**Total Propositions**: {count}
**Atomic Propositions**: {atomic_count}
**Confidence**: {average_confidence}
```

### 5. ANALYZE - Check Completeness

```yaml
operation: analyze
session_id:
  type: string
  required: true
```

**Analysis Checks**:
- Are all propositions properly decomposed?
- Are there contradictions?
- Is the logical structure sound?
- Are dependencies correctly mapped?
- Have we reached sufficient depth?

### 6. REVISE - Refine Propositions

```yaml
operation: revise
session_id:
  type: string
  required: true

proposition_number:
  type: string
  required: true
  description: "The proposition number to revise (e.g., '1.2.3')"

new_content:
  type: string
  required: true
  description: "New content for the proposition"

preserve_children:
  type: boolean
  default: true
  description: "Whether to preserve child propositions during revision"

validate_coherence:
  type: boolean
  default: true
  description: "Whether to validate coherence with children"
```

### 7. UNDO - Reconsider

```yaml
operation: undo
session_id:
  type: string
  required: true

confirm_orphaning:
  type: boolean
  description: "Confirm that orphaning propositions is acceptable"
```

### 8. MOVE - Restructure

```yaml
operation: move
session_id:
  type: string
  required: true

proposition_number:
  type: string
  required: true
  description: "The proposition number to move"

new_parent_number:
  type: string
  required: false
  description: "New parent proposition number (null for root level)"

new_position:
  type: integer
  required: false
  description: "Position among siblings at new location"
```

## Embedded Session State

The skill maintains session state in memory:

```python
tractatus_sessions = {
    'session_id': {
        'concept': str,
        'created_at': timestamp,
        'propositions': {
            '1': {
                'content': str,
                'parent': None,
                'children': ['1.1', '1.2'],
                'depth': 0,
                'is_atomic': False,
                'confidence': 0.5,
                'decomposition_type': None
            },
            '1.1': {
                'content': str,
                'parent': '1',
                'children': [],
                'depth': 1,
                'is_atomic': True,
                'confidence': 0.8,
                'decomposition_type': 'clarification'
            }
        },
        'next_number': 2,
        'max_depth': 5,
        'style': 'analytical'
    }
}
```

## Usage Example

```
START: "What is token optimization?"

Session ID: abc-123
1. Token optimization
  1.1. Reducing input tokens (clarification)
  1.2. Reducing output tokens (clarification)
  1.3. Caching strategies (analysis)

ADD (parent=1.3): "Skill-level caching"
  1.3.1. Lazy loading (cases)
  1.3.2. Selective loading (cases)

ANALYZE:
- Depth: 2 (within limit of 5)
- 7 propositions total
- 1 atomic proposition marked
- Structure: Sound
- Recommendation: Consider decomposing "Token optimization" further

EXPORT (format=markdown):
# Logical Analysis: Token Optimization
[Full markdown tree]
```

## Integration with Skill

When the thinking-review-expert skill validates tractatus thinking:

1. **Without MCP Server**: Uses this embedded implementation
2. **With MCP Server**: Delegates to `mcp__tractatusthinking__tractatus_thinking`

The skill automatically detects which mode to use.

---

**Original Implementation**: `tractatus-thinking/src/index.ts`
**Embedded Version**: Adapted for skill-native execution
**Philosophy**: Based on Wittgenstein's *Tractatus Logico-Philosophicus*
