# Embedded Debug Thinking Tool

**Source**: https://github.com/Alot1z/mcp-server-debug-thinking
**License**: MIT
**Embedded**: v1.0.0

This tool provides graph-based debugging knowledge management WITHOUT requiring the MCP server.

## How It Works (Embedded)

Instead of calling an external MCP server, this skill uses Claude's native capabilities to maintain a debugging knowledge graph:

1. **Graph State**: Maintains nodes and edges in memory
2. **Persistence**: Saves to `~/.debug-thinking-mcp/graph.json`
3. **Querying**: Searches through past debugging sessions
4. **Learning**: Builds knowledge base over time

## Tool Actions

### 1. CREATE - Add Nodes to Graph

```yaml
action: create
nodeType:
  type: string
  enum: [problem, hypothesis, experiment, observation, learning, solution]
  required: true
  description: "Type of node to create"

content:
  type: string
  required: true
  description: "Description of the node"

parentId:
  type: string
  required: false
  description: "Parent node ID for automatic edge creation"

metadata:
  type: object
  required: false
  properties:
    confidence:
      type: number
      minimum: 0
      maximum: 100
      description: "Confidence score (for hypothesis/solution)"
    tags:
      type: array
      items:
        type: string
      description: "Tags for categorization"
    error_type:
      type: string
      description: "Error type (for problem nodes)"
    stack_trace:
      type: string
      description: "Stack trace (for problem nodes)"
    file:
      type: string
      description: "File path (for problem nodes)"
    line:
      type: integer
      description: "Line number (for problem nodes)"
```

**Node Types Explained**:

- **problem**: Error, bug, or issue that needs investigation
- **hypothesis**: Proposed explanation or theory
- **experiment**: Test to verify hypothesis
- **observation**: Result or finding from investigation
- **learning**: Insight or knowledge gained
- **solution**: Fix that resolved the problem

### 2. CONNECT - Link Nodes

```yaml
action: connect
from:
  type: string
  required: true
  description: "Source node ID"

to:
  type: string
  required: true
  description: "Target node ID"

type:
  type: string
  enum: [decomposes, hypothesizes, tests, produces, learns, contradicts, supports, solves]
  required: true
  description: "Type of relationship"

strength:
  type: number
  minimum: 0
  maximum: 1
  required: false
  default: 0.5
  description: "Strength of the relationship"

metadata:
  type: object
  required: false
  description: "Additional metadata for the edge"
```

**Edge Types Explained**:

- **decomposes**: Problem breaks into sub-problems
- **hypothesizes**: Hypothesis explains problem
- **tests**: Experiment tests hypothesis
- **produces**: Experiment produces observation
- **learns**: Observation leads to learning
- **contradicts**: Evidence contradicts hypothesis
- **supports**: Evidence supports hypothesis
- **solves**: Solution solves problem

### 3. QUERY - Search and Analyze

```yaml
action: query
queryType:
  type: string
  enum: [similar-problems, recent-activity]
  required: true
  description: "Type of query"

parameters:
  type: object
  required: false
  properties:
    pattern:
      type: string
      description: "Search pattern for similar-problems query"
    
    limit:
      type: integer
      minimum: 1
      maximum: 100
      default: 10
      description: "Maximum number of results"
    
    minSimilarity:
      type: number
      minimum: 0
      maximum: 1
      default: 0.3
      description: "Minimum similarity score (0-1)"
    
    hours:
      type: integer
      description: "Time window in hours (for recent-activity)"
```

## Embedded Graph Structure

The skill maintains graph state in memory:

```python
debug_graph = {
    'nodes': {
        'node-1': {
            'id': 'node-1',
            'type': 'problem',
            'content': 'TypeError: Cannot read property x of undefined',
            'created_at': '2026-02-06T12:00:00Z',
            'metadata': {
                'error_type': 'TypeError',
                'file': 'src/app.ts',
                'line': 42
            }
        },
        'node-2': {
            'id': 'node-2',
            'type': 'hypothesis',
            'content': 'Missing null check before accessing property',
            'created_at': '2026-02-06T12:01:00Z',
            'metadata': {
                'confidence': 75
            }
        },
        'node-3': {
            'id': 'node-3',
            'type': 'solution',
            'content': 'Add optional chaining operator (?.)',
            'created_at': '2026-02-06T12:05:00Z',
            'metadata': {}
        }
    },
    'edges': [
        {
            'from': 'node-2',
            'to': 'node-1',
            'type': 'hypothesizes',
            'strength': 0.8,
            'created_at': '2026-02-06T12:01:00Z'
        },
        {
            'from': 'node-3',
            'to': 'node-1',
            'type': 'solves',
            'strength': 1.0,
            'created_at': '2026-02-06T12:05:00Z'
        }
    ]
}
```

## Usage Workflow Example

```
Step 1: CREATE problem
- Type: problem
- Content: "classifyHandoffIfNeeded is not defined"
- Metadata: { error_type: "ReferenceError", context: "agent spawn" }

→ Node ID: prob-001

Step 2: CREATE hypothesis
- Type: hypothesis
- Content: "Error occurs with complex multi-circle agent spawns"
- Parent ID: prob-001
- Metadata: { confidence: 85 }

→ Node ID: hyp-001
→ Edge created: hyp-001 --hypothesizes→ prob-001

Step 3: CREATE experiment
- Type: experiment
- Content: "Test simple agent spawn"
- Parent ID: hyp-001

→ Node ID: exp-001
→ Edge created: exp-001 --tests→ hyp-001

Step 4: CREATE observation
- Type: observation
- Content: "Simple agent works fine (a27cab0 succeeded)"
- Parent ID: exp-001

→ Node ID: obs-001
→ Edge created: exp-001 --produces→ obs-001

Step 5: CREATE learning
- Type: learning
- Content: "Complex spawns fail, simple spawns work"
- Parent ID: obs-001

→ Node ID: learn-001

Step 6: CREATE solution
- Type: solution
- Content: "Use sequential single-circle execution instead"
- Parent ID: prob-001

→ Node ID: sol-001
→ Edge created: sol-001 --solves→ prob-001

Step 7: QUERY similar-problems
- Pattern: "classifyHandoffIfNeeded"
- Limit: 5

→ Returns previous instances of this error and their solutions
```

## Similarity Calculation

For `similar-problems` queries:

```python
def calculate_similarity(query, candidate):
    """
    Calculate similarity between query problem and candidate.
    
    Uses: word overlap, error type matching, context similarity
    """
    query_words = set(query.lower().split())
    candidate_words = set(candidate['content'].lower().split())
    
    # Jaccard similarity
    intersection = query_words & candidate_words
    union = query_words | candidate_words
    similarity = len(intersection) / len(union) if union else 0
    
    # Boost for matching error types
    if query.get('error_type') == candidate.get('metadata', {}).get('error_type'):
        similarity += 0.2
    
    # Boost for matching context
    if query.get('context') == candidate.get('metadata', {}).get('context'):
        similarity += 0.1
    
    return min(similarity, 1.0)
```

## Persistence

Graph data persists to disk at `~/.debug-thinking-mcp/graph.json`:

```json
{
  "version": "1.0",
  "last_updated": "2026-02-06T12:00:00Z",
  "nodes": [...],
  "edges": [...],
  "metadata": {
    "total_nodes": 150,
    "total_edges": 230,
    "problems_solved": 85,
    "success_rate": 0.73
  }
}
```

## Query Results Format

### Similar Problems

```json
{
  "query_type": "similar-problems",
  "pattern": "classifyHandoffIfNeeded",
  "results": [
    {
      "node": {
        "id": "prob-123",
        "type": "problem",
        "content": "classifyHandoffIfNeeded is not defined",
        "similarity": 0.95
      },
      "solutions": [
        {
          "id": "sol-124",
          "content": "Use sequential single-circle execution"
        }
      ],
      "related": ["hyp-125", "exp-126", "learn-127"]
    }
  ],
  "count": 3
}
```

### Recent Activity

```json
{
  "query_type": "recent-activity",
  "time_window_hours": 24,
  "results": [
    {
      "node": {...},
      "edges_in": 2,
      "edges_out": 1,
      "last_modified": "2026-02-06T11:30:00Z"
    }
  ],
  "count": 15
}
```

## Integration with Skill

When the thinking-review-expert skill validates debug thinking:

1. **Without MCP Server**: Uses this embedded implementation
2. **With MCP Server**: Delegates to `mcp__debug-thinking__debug_thinking`

The skill automatically detects which mode to use.

## Benefits of Embedded Debug Thinking

1. **No Installation**: Works immediately without MCP server setup
2. **Persistent Learning**: Remembers all debugging sessions
3. **Pattern Recognition**: Finds similar past problems
4. **Knowledge Building**: Creates personal debugging knowledge base
5. **Visualization Ready**: Graph structure can be exported for visualization

---

**Original Implementation**: `mcp-server-debug-thinking/src/index.ts`
**Embedded Version**: Adapted for skill-native execution
**Storage**: `~/.debug-thinking-mcp/graph.json`
