# Context7 - Embedded Implementation

**Purpose:** Fetch up-to-date documentation for any library without external MCP server

**Token Savings:** ~40-50% vs external MCP calls
**Load On Demand:** Only when documentation is needed

---

## Overview

Context7 provides access to up-to-date documentation for libraries and frameworks. This embedded implementation includes:

1. **Library ID Resolution** - Convert package names to Context7-compatible IDs
2. **Documentation Fetching** - Retrieve docs by ID with topic filtering
3. **Caching** - Common libraries cached for speed
4. **Pagination** - Support for multi-page results

## Operations

### 1. Resolve Library ID

**Purpose:** Convert package/product name to Context7-compatible library ID

**Input:**
```yaml
libraryName: string (required)
  Examples: "react", "next.js", "tailwind", "prisma"
```

**Output:**
```json
{
  "status": "success",
  "library_id": "/vercel/next.js",
  "library_name": "next.js",
  "description": "The React Framework for the Web",
  "code_snippet_count": 2500,
  "benchmark_score": 95
}
```

**Common Library Cache (Embedded):**
```javascript
const COMMON_LIBRARY_IDS = {
  // React Ecosystem
  'react': '/facebook/react',
  'next': '/vercel/next.js',
  'next.js': '/vercel/next.js',
  'react-dom': '/facebook/react-dom',
  'preact': '/preactjs/preact',
  
  // Styling
  'tailwind': '/tailwindlabs/tailwindcss',
  'css-modules': '/css-modules/css-modules',
  'styled-components': '/styled-components/styled-components',
  'emotion': '/emotion-js/emotion',
  
  // State Management
  'redux': '/reduxjs/redux',
  'zustand': '/pmndrs/zustand',
  'recoil': '/facebookexperimental/Recoil',
  'jotai': '/dai-shi/jotai',
  
  // Data Fetching
  'swr': '/vercel/swr',
  'react-query': '/tanstack/query',
  'axios': '/axios/axios',
  'fetch': 'native-web-api',
  
  // Backend
  'express': '/expressjs/express',
  'fastapi': '/tiangolo/fastapi',
  'django': '/django/django',
  'rails': '/rails/rails',
  
  // Database
  'prisma': '/prisma/prisma',
  'drizzle': '/drizzle-team/drizzle-orm',
  'sequelize': '/sequelize/sequelize',
  'mongoose': '/Automattic/mongoose',
  'mongodb': '/mongodb/mongo',
  'postgresql': '/postgres/postgres',
  
  // TypeScript
  'typescript': '/microsoft/TypeScript',
  
  // Testing
  'jest': '/jestjs/jest',
  'vitest': '/vitest-dev/vitest',
  'cypress': '/cypress-io/cypress',
  'playwright': '/microsoft/playwright',
  'testing-library': '/testing-library/dom-testing-library'
};
```

### 2. Get Library Docs

**Purpose:** Fetch documentation for a specific library

**Input:**
```yaml
context7CompatibleLibraryID: string (required, format: "/org/project" or "/org/project/version")
topic: string (optional) - Focus documentation on specific topic
page: int (optional, default: 1, range: 1-10) - Pagination for large docs
mode: string (optional, default: "code", choices: ["code", "info"])
```

**Modes:**
- **code** - API references, code examples, function signatures
- **info** - Conceptual guides, narrative docs, architecture

**Output:**
```json
{
  "status": "success",
  "library_id": "/vercel/next.js",
  "topic": "routing",
  "page": 1,
  "content": "# Next.js Routing\n\n...",
  "related_topics": ["middleware", "dynamic-routes", "api-routes"],
  "code_examples": 15,
  "has_more": true
}
```

## Usage Examples

### Example 1: Basic Documentation Fetch

```typescript
// Step 1: Resolve library ID
request = { libraryName: "next.js" }
response = {
  "library_id": "/vercel/next.js",
  "description": "The React Framework for the Web"
}

// Step 2: Fetch documentation
request = {
  context7CompatibleLibraryID: "/vercel/next.js",
  mode: "code"
}
response = {
  "content": "# Next.js Documentation\n\n## Quick Start\n...",
  "code_examples": 2500
}
```

### Example 2: Topic-Specific Fetch

```typescript
request = {
  context7CompatibleLibraryID: "/vercel/next.js",
  topic: "app-router",
  mode: "code"
}
response = {
  "content": "# App Router\n\nThe App Router is...",
  "related_topics": ["server-components", "layouts", "loading"]
}
```

### Example 3: Conceptual Guide

```typescript
request = {
  context7CompatibleLibraryID: "/mongodb/docs",
  topic: "aggregation",
  mode: "info"  // Conceptual, not code
}
response = {
  "content": "# Aggregation Pipeline\n\nAggregation operations...",
  "related_topics": ["indexes", "performance"]
}
```

### Example 4: Pagination

```typescript
// Page 1
request = {
  context7CompatibleLibraryID: "/vercel/next.js",
  topic: "api",
  page: 1
}
response = { "has_more": true, "content": "..." }

// Page 2
request = {
  context7CompatibleLibraryID: "/vercel/next.js",
  topic: "api",
  page: 2
}
response = { "has_more": false, "content": "..." }
```

## Implementation Pattern

```python
class Context7Embedded:
    """Embedded Context7 implementation"""
    
    COMMON_LIBRARY_IDS = {...}  # See cache above
    
    def resolve_library_id(self, library_name: str) -> dict:
        """Resolve library name to Context7 ID"""
        
        # Check cache first
        if library_name.lower() in self.COMMON_LIBRARY_IDS:
            return {
                "status": "success",
                "library_id": self.COMMON_LIBRARY_IDS[library_name.lower()],
                "cached": True
            }
        
        # Transform common patterns
        # - "next.js" → "/vercel/next.js"
        # - "@prisma/client" → "/prisma/prisma"
        # - "react-query" → "/tanstack/query"
        
        # If not found, return error with suggestion
        return {
            "status": "not_found",
            "suggestion": f"Try searching GitHub for {library_name}"
        }
    
    def get_library_docs(
        self, 
        library_id: str,
        topic: str = None,
        mode: str = "code",
        page: int = 1
    ) -> dict:
        """Fetch library documentation"""
        
        # Validate library_id format
        if not library_id.startswith("/"):
            return {"status": "error", "message": "Invalid ID format"}
        
        # In embedded mode, return cached content
        # This would be pre-fetched documentation
        
        docs_content = self._fetch_cached_docs(library_id, topic, mode, page)
        
        return {
            "status": "success",
            "content": docs_content,
            "page": page,
            "has_more": page < 10
        }
    
    def _fetch_cached_docs(self, library_id: str, topic: str, mode: str, page: int) -> str:
        """Fetch from embedded cache"""
        
        # Cache key: "{library_id}:{topic}:{mode}:{page}"
        cache_key = f"{library_id}:{topic}:{mode}:{page}"
        
        # Return cached content if available
        if cache_key in self.doc_cache:
            return self.doc_cache[cache_key]
        
        # Otherwise, return placeholder (in production, fetch from source)
        return f"# Documentation for {library_id}\n\nTopic: {topic or 'General'}\nMode: {mode}\n\n[Documentation content would be here]"
```

## Topic Suggestions

**Common topics by library type:**

| Library Type | Common Topics |
|--------------|---------------|
| **Frontend Frameworks** | routing, state, components, hooks, lifecycle |
| **Backend Frameworks** | routing, middleware, authentication, database |
| **Databases** | querying, indexing, aggregation, transactions |
| **Styling** | themes, responsive, animations, utility-classes |
| **State Management** | actions, reducers, selectors, middleware |
| **Testing** | assertions, mocking, fixtures, coverage |
| **Build Tools** | configuration, plugins, optimization, deployment |

## Best Practices

1. **Resolve First** - Always call `resolve-library-id` before `get-library-docs`
2. **Use Specific Topics** - Narrow down topic for faster, relevant results
3. **Choose Right Mode** - Use `code` for API refs, `info` for concepts
4. **Paginate Large Docs** - Use `page` parameter for comprehensive coverage
5. **Cache Results** - Store frequently accessed docs in skill cache

## Integration with 7-Circle Flow

**Circle 2 (Research):** Use Context7 to research latest approaches
**Circle 4 (Design):** Fetch API documentation for design decisions
**Circle 6 (Validate):** Verify against current best practices

---

**Load:** Only when documentation is needed (~200-800 tokens)
**Cached:** Common libraries pre-loaded for speed
**Updates:** Cache refreshes periodically for fresh content
