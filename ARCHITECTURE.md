# Thinking Review Expert - Architecture Documentation

## System Architecture Overview

```mermaid
graph TB
    subgraph "Input Layer"
        A[Thinking Session Output]
        B[Agent Report]
        C[Cognitive Process]
    end
    
    subgraph "Processing Layer"
        D[Sequential Thinking Validator]
        E[Tractatus Thinking Validator]
        F[Debug Thinking Validator]
    end
    
    subgraph "Enhancement Layer"
        G[Stop-Slop Filter]
        H[Beautiful-Mermaid Generator]
        I[Diagram Auto-Generator]
    end
    
    subgraph "Output Layer"
        J[Validation Report]
        K[Mermaid Diagrams]
        L[Enhanced Documentation]
    end
    
    A --> D
    B --> E
    C --> F
    D --> G
    E --> G
    F --> G
    G --> H
    H --> I
    I --> J
    I --> K
    I --> L
```

## Component Architecture

### 1. Core Validation Engine

```mermaid
graph LR
    subgraph "Validation Engine"
        A[Parser] --> B[Analyzer]
        B --> C[Validator]
        C --> D[Scorer]
        D --> E[Reporter]
    end
    
    subgraph "Checklist System"
        F[7-Circle Checklist]
        G[Logic Checklist]
        H[Depth Checklist]
        I[Enhancement Plan]
    end
    
    B --> F
    B --> G
    C --> H
    D --> I
```

### 2. Embedded Tools Architecture

```mermaid
graph TB
    subgraph "Embedded Tools (Priority 1)"
        A[Sequential Thinking]
        B[Tractatus Thinking]
        C[Debug Thinking]
    end
    
    subgraph "MCP References (Priority 2)"
        D[Context7 MCP]
        E[DeepWiki MCP]
    end
    
    subgraph "Tool Router"
        F[Priority Selector]
        G[Fallback Handler]
    end
    
    F --> A
    F --> B
    F --> C
    F --> D
    F --> E
    A --> G
    D --> G
```

### 3. Auto-Diagram Generation System

```mermaid
sequenceDiagram
    participant T as Thinking Tool
    participant M as Mermaid Generator
    participant D as Diagram Engine
    participant O as Output
    
    T->>M: Capture thinking state
    M->>D: Parse cognitive patterns
    D->>D: Apply beautiful-mermaid styling
    D->>O: Generate diagram
    O->>O: Embed in report
```

## Data Flow Architecture

### Input Processing Flow

```mermaid
flowchart TD
    A[Raw Input] --> B{Input Type?}
    B -->|Sequential| C[Seq Validator]
    B -->|Tractatus| D[Tractatus Validator]
    B -->|Debug| E[Debug Validator]
    B -->|7-Circle| F[Full Validator]
    
    C --> G[Apply Stop-Slop]
    D --> G
    E --> G
    F --> G
    
    G --> H[Generate Mermaid]
    H --> I[Score Results]
    I --> J[Create Report]
    J --> K[Output]
```

### Enhancement Pipeline

```mermaid
graph LR
    subgraph "Stage 1: Analysis"
        A[Parse] --> B[Validate]
        B --> C[Score]
    end
    
    subgraph "Stage 2: Enhancement"
        C --> D[Stop-Slop]
        D --> E[Mermaid Gen]
        E --> F[Style Apply]
    end
    
    subgraph "Stage 3: Output"
        F --> G[Format]
        G --> H[Package]
    end
```

## File Structure

```
thinking-review-expert/
├── skill.md                    # Main skill entry point
├── package.json                # NPM package configuration
├── README.md                   # User documentation
├── ARCHITECTURE.md             # This file
├── LICENSE                     # MIT License
│
├── references/                 # Validation checklists
│   ├── 7circle-checklist.md
│   ├── logical-structure-checklist.md
│   ├── depth-quality-checklist.md
│   ├── enhancement-plan.md
│   ├── stop-slop.md
│   └── beautiful-mermaid.md
│
├── tools/                      # Embedded tool implementations
│   ├── embedded-sequential-thinking.md
│   ├── embedded-tractatus-thinking.md
│   ├── embedded-debug-thinking.md
│   ├── embedded-context7.md
│   ├── embedded-deepwiki.md
│   └── README.md
│
├── docs/                       # Additional documentation
│   ├── GITHUB_PAGES.md
│   └── CODE_REVIEW_ROADMAP.md
│
└── agents/                     # Agent configurations
    └── agent.yaml
```

## Integration Points

### Claude Code Integration

```mermaid
graph LR
    subgraph "Claude Code"
        A[User Command]
        B[Skill Loader]
        C[Context Manager]
    end
    
    subgraph "Thinking Review Expert"
        D[skill.md]
        E[References]
        F[Tools]
    end
    
    A --> B
    B --> C
    C --> D
    D --> E
    D --> F
```

### MCP Server Integration

```mermaid
graph TB
    subgraph "Skill"
        A[Tool Request]
    end
    
    subgraph "MCP Layer"
        B[Context7 Server]
        C[DeepWiki Server]
        D[Sequential Server]
        E[Tractatus Server]
        F[Debug Server]
    end
    
    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
```

## Performance Optimization

### Token Saving Strategy

```mermaid
graph LR
    A[Load skill.md] --> B{Need Reference?}
    B -->|Yes| C[Load Specific Reference]
    B -->|No| D[Continue]
    C --> D
    D --> E{Need Tool?}
    E -->|Yes| F[Load Embedded Tool]
    E -->|No| G[Execute]
    F --> G
```

### Lazy Loading Architecture

1. **Base Load** (~100 tokens): skill.md metadata
2. **On-Demand References**: Load checklists only when needed
3. **Embedded Tools**: Load only when specific tool required
4. **MCP Fallback**: Use MCP servers only if embedded unavailable

## Extension Points

### Adding New Validators

```mermaid
graph TB
    A[Create Validator] --> B[Define Checklist]
    B --> C[Add to References]
    C --> D[Update skill.md]
    D --> E[Register Priority]
    E --> F[Test Integration]
```

### Adding New Diagram Types

```mermaid
graph LR
    A[Define Diagram Pattern] --> B[Add to beautiful-mermaid]
    B --> C[Create Template]
    C --> D[Register Generator]
    D --> E[Test Output]
```

## Security Considerations

### Input Validation

```mermaid
flowchart TD
    A[Input Received] --> B{Validate Source}
    B -->|Known| C[Process]
    B -->|Unknown| D{Sanitize}
    D --> E[Remove unsafe]
    E --> C
    C --> F[Output]
```

### Tool Execution Safety

1. **Sandboxed Execution**: All embedded tools run in isolation
2. **Resource Limits**: Timeout and memory constraints
3. **Fallback Handling**: Graceful degradation on errors
4. **Input Sanitization**: Clean all inputs before processing

## Quality Assurance

### Testing Strategy

```mermaid
graph TB
    subgraph "Unit Tests"
        A[Validator Tests]
        B[Tool Tests]
        C[Diagram Tests]
    end
    
    subgraph "Integration Tests"
        D[MCP Integration]
        E[Claude Code Integration]
        F[End-to-End Flow]
    end
    
    subgraph "Quality Checks"
        G[Token Efficiency]
        H[Performance]
        I[Accuracy]
    end
    
    A --> D
    B --> D
    C --> D
    D --> G
    D --> H
    D --> I
```

### Continuous Improvement

```mermaid
cycle-arrow
    A[Collect Metrics] --> B[Analyze Patterns]
    B --> C[Identify Issues]
    C --> D[Implement Fixes]
    D --> E[Test Changes]
    E --> A
```

---

**Version**: 6.0.0  
**Last Updated**: 2025-01-06  
**Maintainer**: Alot1z
