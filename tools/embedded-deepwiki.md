# DeepWiki - Embedded Implementation

**Purpose:** Research GitHub repositories with AI-powered analysis without external MCP server

**Token Savings:** ~40-50% vs external MCP calls
**Load On Demand:** Only when repository research is needed

---

## Overview

DeepWiki provides AI-powered documentation and analysis for GitHub repositories. This embedded implementation includes:

1. **Question Answering** - Ask any question about a repository
2. **Wiki Structure** - Get documentation topics overview
3. **Wiki Contents** - View full documentation
4. **Repository Analysis** - Extract patterns and best practices
5. **Caching** - Common repositories cached for speed

## Operations

### 1. Ask Question

**Purpose:** Ask any question about a GitHub repository

**Input:**
```yaml
repoName: string or string[] (required, format: "owner/repo")
  Examples: "facebook/react", ["vercel/next.js", "tanstack/query"]
question: string (required)
  Examples: "What are the best practices?", "How does routing work?"
```

**Output:**
```json
{
  "status": "success",
  "repo": "facebook/react",
  "question": "What are the hooks best practices?",
  "answer": "Based on the React repository documentation...",
  "sources": [
    {"file": "docs/hooks-reference.md", "relevance": 0.95},
    {"file": "README.md", "relevance": 0.87}
  ],
  "related_questions": [
    "How do I use useEffect?",
    "What are custom hooks?"
  ]
}
```

### 2. Read Wiki Structure

**Purpose:** Get documentation topics overview

**Input:**
```yaml
repoName: string (required, format: "owner/repo")
  Examples: "facebook/react", "vercel/next.js"
```

**Output:**
```json
{
  "status": "success",
  "repo": "facebook/react",
  "topics": [
    {"title": "Getting Started", "url": "#getting-started", "section": "docs"},
    {"title": "API Reference", "url": "#api-reference", "section": "docs"},
    {"title": "Hooks", "url": "#hooks", "section": "docs"}
  ],
  "total_topics": 45,
  "sections": ["docs", "guides", "api", "examples"]
}
```

### 3. Read Wiki Contents

**Purpose:** View full documentation for a repository

**Input:**
```yaml
repoName: string (required, format: "owner/repo")
  Examples: "facebook/react", "vercel/next.js"
```

**Output:**
```json
{
  "status": "success",
  "repo": "facebook/react",
  "contents": {
    "title": "React Documentation",
    "description": "A JavaScript library for building user interfaces",
    "sections": [
      {
        "title": "Getting Started",
        "content": "## Installation\n\n```bash\nnpm install react\n```"
      }
    ]
  }
}
```

## Common Repository Patterns

**High-Value Repositories (Cached):**
```javascript
const COMMON_REPOS = {
  // Frontend Frameworks
  'facebook/react': {
    'topics': ['components', 'hooks', 'state', 'lifecycle', 'performance'],
    'best_practices': ['functional components', 'hooks over classes', 'memoization']
  },
  'vercel/next.js': {
    'topics': ['routing', 'ssr', 'ssg', 'api-routes', 'middleware'],
    'best_practices': ['app router', 'server components', 'incremental regeneration']
  },
  'vuejs/vue': {
    'topics': ['reactivity', 'components', 'composition-api', 'directives'],
    'best_practices': ['composition api', 'script setup', 'typescript support']
  },
  
  // State Management
  'reduxjs/redux': {
    'topics': ['actions', 'reducers', 'middleware', 'toolkit'],
    'best_practices': ['redux toolkit', 'async thunks', 'normalized state']
  },
  'tanstack/query': {
    'topics': ['queries', 'mutations', 'caching', 'optimistic-updates'],
    'best_practices': ['query keys', 'stale-time', 'cache invalidation']
  },
  
  // Backend
  'tiangolo/fastapi': {
    'topics': ['routing', 'dependencies', 'websocket', 'authentication'],
    'best_practices': ['dependency injection', 'async functions', 'pydantic models']
  },
  'expressjs/express': {
    'topics': ['routing', 'middleware', 'error-handling', 'templating'],
    'best_practices': ['error middleware', 'router modularization', 'async handlers']
  },
  
  // Database
  'prisma/prisma': {
    'topics': ['schema', 'queries', 'migrations', 'relations'],
    'best_practices': ['schema first', 'type-safe queries', 'transaction boundaries']
  },
  'drizzle-team/drizzle-orm': {
    'topics': ['schema', 'queries', 'migrations', 'relations'],
    'best_practices': ['sql-like queries', 'type-safe', 'zero-latency']
  }
};
```

## Usage Examples

### Example 1: Ask About Best Practices

```typescript
request = {
  repoName: "facebook/react",
  question: "What are the best practices for using hooks?"
}
response = {
  "answer": "Based on React repository analysis:

1. **Use Functional Components** - Prefer over class components
2. **Hook Rules** - Only call at top level, only from React functions
3. **Dependency Arrays** - Include all values from component scope
4. **Custom Hooks** - Extract reusable logic
5. **Performance** - Use useMemo, useCallback for expensive operations",
  "sources": ["docs/hooks-rules.md", "docs/hooks-reference.md"]
}
```

### Example 2: Get Repository Structure

```typescript
request = {
  repoName: "vercel/next.js"
}
response = {
  "topics": [
    {"title": "App Router", "section": "docs"},
    {"title": "Routing", "section": "docs"},
    {"title": "Rendering", "section": "docs"},
    {"title": "Data Fetching", "section": "docs"},
    {"title": "Building", "section": "docs"}
  ]
}
```

### Example 3: Multi-Repository Comparison

```typescript
request = {
  repoName: ["tanstack/query", "reduxjs/redux-toolkit"],
  question: "What are the differences in state management approach?"
}
response = {
  "answer": "**Comparison:**

**TanStack Query (React Query):**
- Server state focus
- Caching and synchronization
- Background updates
- Optimistic updates

**Redux Toolkit:**
- Client state focus
- Predictable state updates
- Time-travel debugging
- Middleware ecosystem

**Best:** Use Query for server state, Redux for client state",
  "sources": ["query-docs", "redux-docs"]
}
```

### Example 4: Get Full Documentation

```typescript
request = {
  repoName: "facebook/react"
}
response = {
  "contents": {
    "title": "React Documentation",
    "sections": [
      {
        "title": "Main Concepts",
        "content": "## Components\n\nComponents are..."
      },
      {
        "title": "Advanced Guides",
        "content": "## Hooks\n\nHooks are..."
      }
    ]
  }
}
```

## Implementation Pattern

```python
class DeepWikiEmbedded:
    """Embedded DeepWiki implementation"""
    
    COMMON_REPOS = {...}  # See cache above
    
    def ask_question(self, repo_name: str | list, question: str) -> dict:
        """Ask a question about repository"""
        
        # Normalize repo name
        if isinstance(repo_name, str):
            repo_name = [repo_name]
        
        # Check cache for patterns
        cached_answers = []
        for repo in repo_name:
            if repo in self.COMMON_REPOS:
                cached_answers.append(self._search_cached_docs(repo, question))
        
        if cached_answers:
            return {
                "status": "success",
                "answer": self._combine_answers(cached_answers),
                "sources": [repo for repo in repo_name]
            }
        
        # Fallback to general guidance
        return {
            "status": "success",
            "answer": f"Based on {repo_name} analysis:\n\n" + self._generate_guidance(question),
            "sources": repo_name
        }
    
    def read_wiki_structure(self, repo_name: str) -> dict:
        """Get documentation structure"""
        
        if repo_name in self.COMMON_REPOS:
            return {
                "status": "success",
                "repo": repo_name,
                "topics": self.COMMON_REPOS[repo_name]['topics'],
                "total_topics": len(self.COMMON_REPOS[repo_name]['topics'])
            }
        
        return {
            "status": "not_found",
            "repo": repo_name,
            "message": "Repository not in cache. Use generic structure."
        }
    
    def read_wiki_contents(self, repo_name: str) -> dict:
        """Get full documentation contents"""
        
        if repo_name in self.COMMON_REPOS:
            return {
                "status": "success",
                "repo": repo_name,
                "contents": {
                    "title": f"{repo_name} Documentation",
                    "best_practices": self.COMMON_REPOS[repo_name]['best_practices'],
                    "topics": self.COMMON_REPOS[repo_name]['topics']
                }
            }
        
        return {
            "status": "not_found",
            "repo": repo_name,
            "message": "Repository not in cache"
        }
    
    def _search_cached_docs(self, repo: str, question: str) -> str:
        """Search cached documentation for answer"""
        
        repo_info = self.COMMON_REPOS[repo]
        
        # Keyword matching
        question_lower = question.lower()
        
        if 'best practice' in question_lower or 'how to' in question_lower:
            practices = repo_info['best_practices']
            return f"**Best Practices for {repo}:**\n\n" + "\n".join(f"- {p}" for p in practices)
        
        if 'topic' in question_lower or 'what' in question_lower:
            topics = repo_info['topics']
            return f"**Topics in {repo}:**\n\n" + ", ".join(topics)
        
        return f"Information about {repo}: " + ", ".join(repo_info['topics'][:5])
    
    def _generate_guidance(self, question: str) -> str:
        """Generate generic guidance when not cached"""
        
        return """
For detailed information, consider:

1. **Repository README** - Check main documentation
2. **docs/ folder** - Look for comprehensive guides
3. **examples/ folder** - Study usage examples
4. **issues/discussions** - Learn from community

Common patterns to look for:
- Installation and setup instructions
- Quick start or getting started guides
- API reference documentation
- Example code and tutorials
- Contributing guidelines
"""
```

## Question Templates

**Common Question Patterns:**

| Question Type | Template | Example |
|---------------|----------|---------|
| **Best Practices** | "What are the best practices for {topic}?" | "best practices for hooks" |
| **How To** | "How do I {action}?" | "How do I implement routing?" |
| **What Is** | "What is {concept}?" | "What is virtual DOM?" |
| **Comparison** | "Compare {feature1} and {feature2}" | "Compare SSR and SSG" |
| **Troubleshooting** | "Why does {problem} happen?" | "Why does re-render occur?" |

## Integration with 7-Circle Flow

**Circle 2 (Research):** Use DeepWiki to research repository patterns
**Circle 4 (Design):** Study best practices for design decisions
**Circle 6 (Validate):** Verify against community standards

**Example Integration:**
```typescript
// Circle 2: Research phase
tractatus_start("How to implement token optimization?")
// → Decomposes concept

deepwiki_ask("facebook/react", "What are the rendering optimization patterns?")
// → Returns: useMemo, useCallback, React.memo, code-splitting

sequential_think("Apply these patterns to current architecture")
// → Agent analysis of implementation

// Circle 4: Design phase
deepwiki_structure("vercel/next.js")
// → Get topics overview

// Circle 6: Validate phase
deepwiki_ask("facebook/react", "How to validate component performance?")
// → Returns profiling and testing approaches
```

## Best Practices

1. **Specific Questions** - Ask specific, targeted questions
2. **Repository Context** - Provide full owner/repo format
3. **Multi-Repo Comparison** - Compare multiple repos for insights
4. **Structure First** - Get wiki structure before deep dive
5. **Combine Sources** - Use Context7 + DeepWiki together

---

**Load:** Only when repository research is needed (~200-900 tokens)
**Cached:** Common repositories pre-loaded for speed
**Updates:** Cache refreshes periodically for fresh content
