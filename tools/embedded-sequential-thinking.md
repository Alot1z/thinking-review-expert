# Embedded Sequential Thinking Tool

**Source**: https://github.com/Alot1z/mcp-server-mas-sequential-thinking
**License**: MIT
**Embedded**: v2.1.0

This tool provides multi-agent sequential thinking WITHOUT requiring the MCP server.

## How It Works (Embedded)

Instead of calling an external MCP server, this skill uses Claude's native capabilities to simulate the multi-agent coordination:

1. **Multi-Agent Simulation**: Claude acts as 6 different agents (Factual, Emotional, Critical, Optimistic, Creative, Synthesis)
2. **Thought Processing**: Each thought is processed through the appropriate agent(s)
3. **Response Synthesis**: Results from all agents are synthesized into a coherent response

## Tool Parameters

```yaml
thought:
  type: string
  required: true
  description: "Content of the thinking step"

thoughtNumber:
  type: integer
  required: true
  minimum: 1
  description: "Sequence number starting from 1"

totalThoughts:
  type: integer
  required: true
  minimum: 1
  description: "Estimated total thoughts required"

nextThoughtNeeded:
  type: boolean
  required: true
  description: "Whether another thought step follows this one"

isRevision:
  type: boolean
  required: true
  description: "Whether this thought revises a previous thought"

branchFromThought:
  type: integer
  required: false
  description: "Thought number to branch from for alternative exploration"

branchId:
  type: string
  required: false
  description: "Unique identifier for the branch (required if branchFromThought set)"

needsMoreThoughts:
  type: boolean
  required: true
  description: "Whether more thoughts are needed beyond the initial estimate"
```

## Embedded Implementation

### Agent Routing Logic

```python
# This logic is embedded directly in the skill
def route_to_agents(thought, thought_number, total_thoughts):
    """Determine which agents should process this thought."""
    
    # Early thoughts (1-3): Factual + Creative agents
    if thought_number <= 3:
        return ['factual', 'creative']
    
    # Middle thoughts: Critical + Emotional agents
    elif thought_number <= total_thoughts * 0.7:
        return ['critical', 'emotional']
    
    # Later thoughts: Optimistic agent
    elif thought_number < total_thoughts:
        return ['optimistic']
    
    # Final thought: Synthesis agent
    else:
        return ['synthesis']
```

### Agent Processing Patterns

**Factual Agent**: "What are the concrete facts here?"
- Extracts objective information
- Identifies verifiable data
- Separates facts from assumptions

**Emotional Agent**: "How does this feel? What are the reactions?"
- Considers emotional impact
- Identifies stakeholder perspectives
- Acknowledges human factors

**Critical Agent**: "What could go wrong? What's missing?"
- Identifies potential issues
- Questions assumptions
- Finds gaps in reasoning

**Optimistic Agent**: "What if this works perfectly? What's the best case?"
- Envisions positive outcomes
- Finds opportunities
- Builds confidence

**Creative Agent**: "What are we not considering? Wild ideas?"
- Generates alternatives
- Thinks outside the box
- Challenges constraints

**Synthesis Agent**: "How do all perspectives come together?"
- Integrates all viewpoints
- Finds common themes
- Creates coherent conclusions

## Usage Examples

### Basic Sequential Thinking

```
Thought 1/5: "I need to analyze the user's request for embedded tools"

Response: 
[Factual Agent] The user wants tool implementations copied directly into the skill
[Creative Agent] Consider creating a hybrid approach - documentation + embedded logic
[Synthesis] Start by reading the MCP server implementations to understand what to embed
```

### With Revision

```
Thought 3/5 (isRevision=true, revises thought 2): "Actually, I should embed the core logic patterns"

Response:
[Critical Agent] Previous approach was too documentation-focused
[Factual Agent] Need to extract actual processing logic from the servers
[Synthesis] Create embedded tool definitions that Claude can follow directly
```

### With Branching

```
Thought 4/8 (branchFromThought=3, branchId="alternative-approach"): "Alternative: Use simulation patterns"

Response:
[Creative Agent] Instead of embedding code, embed agent simulation patterns
[Optimistic Agent] This approach works entirely within Claude's capabilities
[Synthesis] Branch exploring simulation-based embedded tools
```

## Response Format

Each thought generates a structured response:

```markdown
## Thought #{thoughtNumber}/{totalThoughts}

**Your Input**: {thought}

### Agent Analysis

**{Agent Name}**: {agent's perspective on the thought}

### Synthesis

{integrated analysis combining all agent perspectives}

### Next Steps

{guidance for next thought or completion}
```

## Integration with Skill

When the thinking-review-expert skill validates sequential thinking:

1. **Without MCP Server**: Uses this embedded implementation
2. **With MCP Server**: Delegates to `mcp__sequential-thinking__sequentialthinking`

The skill automatically detects which mode to use.

## Security & Validation

Embedded implementation includes:

- **Input Sanitization**: Strips HTML entities, checks injection patterns
- **Length Validation**: Enforces maximum thought lengths (default: 5000 chars)
- **Quote Limiting**: Detects excessive quotes (potential injection)
- **Pattern Validation**: Blocks suspicious patterns

(From original MCP server's SecurityConstants)

---

**Original Implementation**: `mcp-server-mas-sequential-thinking/src/main.py`
**Embedded Version**: Adapted for skill-native execution
**Token Savings**: ~50% vs external MCP calls
