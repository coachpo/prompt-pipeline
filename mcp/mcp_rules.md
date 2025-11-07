# MCP Calling Rules

## Purpose

This document defines the governance rules for calling Model Context Protocol (MCP) servers. It serves as the authoritative guide on **when**, **how**, and **under what constraints** MCP servers should be invoked.

**Server Registry**: For detailed server capabilities and API specifications, consult [`mcp_registry.md`](./mcp_registry.md).

---

## Table of Contents

- [When to Use MCP](#when-to-use-mcp)
- [How to Use MCP](#how-to-use-mcp)
- [Fallback Strategies](#fallback-strategies)
- [Should and Shouldn't](#should-and-shouldnt)
- [Tool Call Report](#tool-call-report)

---

## When to Use MCP

### Decision Tree

```
Is the task achievable with local/offline tools?
├─ YES → Use local tools (grep, LSP, file ops, etc.)
└─ NO  → Proceed to MCP evaluation
          ↓
          Does the task require external knowledge/automation?
          ├─ YES → Consult registry (mcp_registry.md)
          │        ↓
          │        Select appropriate MCP based on:
          │        - Functional category match
          │        - Priority matrix (when overlap exists)
          │        - Scope minimization capability
          └─ NO  → Manual investigation or request user clarification
```

### Triggers for MCP Usage

**Code Operations**:

- Codebase structure analysis exceeding file-level scope
- Symbol/reference tracking across multiple files
- Batch refactoring with pattern-based transformations
- Execution of non-interactive shell commands (tests, builds, lints)

**Knowledge Retrieval**:

- Official documentation verification (API signatures, version differences)
- Community best practices and architectural patterns
- Recent discussions, issues, or release announcements
- Project-specific wiki knowledge

**Planning & Coordination**:

- Multi-step task decomposition (6-10 steps)
- Task tracking with dependency management
- Long-term knowledge persistence across sessions

**Automation & Testing**:

- Browser-based end-to-end workflows
- Visual regression testing
- System-level file/process operations
- Design token extraction and asset export

**UI Development**:

- Design system verification and token extraction
- Component registry queries and scaffolding

### Consultation Priority

1. **Check Local Capabilities First**: grep, file readers, language servers, IDE tools
2. **Consult Registry**: Review [`mcp_registry.md`](./mcp_registry.md) for server selection
3. **Apply Priority Matrix**: When multiple servers apply, use documented priority rules
4. **Verify Prerequisites**: Ensure network access, credentials, and permissions

---

## How to Use MCP

### Core Principles

#### 1. Prudent Single Selection

- **Maximum 1 MCP per Round**: Limit to one MCP service per interaction
- **Offline First**: Exhaust local tools before external calls
- **Explicit Justification**: Document reason and expected outcome before invocation

#### 2. Sequential Invocation

- **Strict Serial Execution**: No parallel MCP calls within a single round
- **Multi-Round Decomposition**: Complex workflows span multiple interactions
- **State Preservation**: Carry forward results to inform subsequent calls

#### 3. Minimal Scope

- **Precise Parameters**: Define exact boundaries (paths, globs, token limits)
- **Filter Aggressively**: Use include/exclude patterns to reduce noise
- **Bounded Queries**: Limit result sets to actionable sizes

#### 4. Traceability

- **Pre-Call Documentation**: State intent, parameters, and expected output
- **Post-Call Reporting**: Append standardized report (see [Tool Call Report](#tool-call-report))
- **Status Tracking**: Mark as Success/Retry/Fallback

### Calling Workflow

#### Phase 1: Preparation

```
1. Identify task requirements and constraints
2. Consult mcp_registry.md for candidate servers
3. Apply priority matrix if multiple servers match
4. Verify prerequisites (credentials, network, permissions)
5. Define precise parameters and scope
```

#### Phase 2: Execution

```
1. State invocation intent explicitly
2. Call MCP server with minimal scope
3. Monitor for errors (rate limits, timeouts, empty results)
4. Apply retry logic if applicable (see Fallback Strategies)
```

#### Phase 3: Post-Processing

```
1. Validate results against expected output
2. Append Tool Call Report
3. Determine if additional rounds needed
4. Update persistent knowledge if applicable (Memory server)
```

### Parameter Optimization

| Aspect               | Guideline                            | Example                                     |
| -------------------- | ------------------------------------ | ------------------------------------------- |
| **Path Scoping**     | Constrain to relevant subdirectories | `path="src/api"` not `path="."`             |
| **Pattern Matching** | Use globs to include/exclude         | `paths_include_glob="**/*.ts"`              |
| **Token Limits**     | Respect server-specific constraints  | `tokens=3000` (Context7: ≤5000)             |
| **Result Limits**    | Cap at actionable size               | `limit=20` not `limit=1000`                 |
| **Time Bounds**      | Use recency filters for searches     | `recencyDays=7` for recent issues           |
| **Domain Filters**   | Whitelist official sources           | `domains=["python.org", "docs.python.org"]` |

### Typical Patterns

Refer to [`mcp_registry.md`](./mcp_registry.md) for server-specific API patterns. General workflow archetypes:

1. **Analysis → Edit → Validate**

   - Understand structure → Locate targets → Apply changes → Run tests

2. **Documentation Cascade**

   - Official docs → Community wiki → Web search → User clarification

3. **Plan → Track → Execute → Verify**

   - Generate plan → Create tasks → Implement steps → Quality gate

4. **Design → Scaffold → Implement → Test**
   - Extract tokens → Query components → Generate code → E2E validation

---

## Fallback Strategies

### Error Handling

| Error Type           | Detection                  | Action                             | Backoff | Max Retries |
| -------------------- | -------------------------- | ---------------------------------- | ------- | ----------- |
| **429 Rate Limit**   | HTTP 429 status            | Reduce scope, narrow parameters    | 20s     | 1           |
| **5xx Server Error** | HTTP 5xx status            | Retry with same parameters         | 2s      | 1           |
| **Timeout**          | No response within limit   | Retry with reduced scope           | 2s      | 1           |
| **Empty Results**    | Zero hits or null response | Narrow query OR request user hints | -       | 0           |
| **Network Failure**  | Connection refused/timeout | Switch to offline fallback         | -       | 0           |

### Degradation Chain

```
Primary MCP Server
  ↓ (failure/unavailable)
Secondary Alternative (consult Priority Matrix)
  ↓ (failure/unavailable)
Local Offline Tools (grep, LSP, file ops)
  ↓ (insufficient)
Conservative Answer + Uncertainty Annotation
  ↓ (still blocked)
Request User Clarification or Additional Context
```

### Fallback Examples

**Documentation Retrieval**:

```
Context7 (official docs)
  → DeepWiki (community wiki)
    → DuckDuckGo (web search, official domains)
      → Request user for specific sources
```

**Code Operations**:

```
Serena (LSP-based analysis)
  → Local grep/ripgrep
    → Manual file inspection
      → Request user for specific files
```

**System Operations**:

```
Desktop Commander (full capabilities)
  → Read-only information gathering
    → Offline file system reasoning
      → Request user for manual verification
```

### When to Abort

Stop and request user guidance when:

- Credentials missing or expired
- Network explicitly restricted
- Query contains sensitive data (keys, tokens, PII)
- Task requires destructive automation without explicit approval
- Retry limit exhausted without progress
- Scope ambiguity prevents meaningful results

---

## Should and Shouldn't

### ✅ Should

**General**:

- Consult `mcp_registry.md` before every MCP invocation
- Document intent and expected output before calling
- Apply minimal scope (paths, globs, token limits)
- Append Tool Call Report after every invocation
- Prefer local tools when sufficient

**Parameter Discipline**:

- Use precise path/glob filters
- Respect server-specific token constraints
- Limit result sets to actionable sizes
- Apply temporal filters (recencyDays) when appropriate
- Whitelist official domains for web searches

**Error Handling**:

- Implement backoff for rate limits and server errors
- Narrow scope on retries
- Follow degradation chain systematically
- Annotate uncertainty when falling back to offline answers
- Request user clarification when blocked

**Knowledge Management**:

- Persist key decisions and learnings (Memory server)
- Query historical context before new implementations
- Update task status immediately after completion
- Link related concepts and decisions

### ❌ Shouldn't

**Never**:

- Call multiple MCP servers in parallel within one round
- Make speculative calls "just in case"
- Expose internal reasoning process to user (Sequential Thinking)
- Automate production systems without explicit authorization
- Submit queries containing credentials, keys, or PII
- Perform full-project scans without path filters
- Retry indefinitely without narrowing scope
- Ignore Priority Matrix when server overlap exists

**Avoid**:

- Calling MCP when local tools suffice
- Using default/broad parameters (e.g., `path="."`)
- Requesting large result sets (e.g., `limit=1000`)
- Skipping Tool Call Report documentation
- Making calls without consulting registry first
- Ignoring server-specific constraints (token limits)
- Proceeding when network explicitly restricted

**Anti-Patterns**:

- **Shotgun Approach**: Calling multiple MCPs hoping one works
- **Scope Creep**: Using broad parameters to "see what's there"
- **Silent Failures**: Not reporting errors or fallback actions
- **Speculation**: Calling MCPs without concrete need
- **Duplication**: Re-querying instead of checking Memory server

---

## Tool Call Report

### Format

Append this section at the end of **every response** involving MCP invocations:

```markdown
【MCP Call Report】
Service: <server-name>
Trigger: <specific reason for invocation>
Parameters: <key parameter summary>
Result: <hit count / main sources / output summary>
Status: <Success | Retry | Fallback>
```

### Field Definitions

| Field          | Description                   | Example                                                              |
| -------------- | ----------------------------- | -------------------------------------------------------------------- |
| **Service**    | MCP server name from registry | `Serena`, `Context7`, `Sequential Thinking`                          |
| **Trigger**    | Concrete reason for call      | `Locate all usages of deprecated function`                           |
| **Parameters** | Key parameters (concise)      | `find_referencing_symbols(symbol="oldApi", path="src/")`             |
| **Result**     | Outcome summary               | `23 references found across 8 files`                                 |
| **Status**     | Execution status              | `Success`, `Retry (429, reduced scope)`, `Fallback (timeout → grep)` |

### Examples

**Success**:

```
【MCP Call Report】
Service: Serena
Trigger: Locate all references to deprecated function `calculateTotal`
Parameters: find_referencing_symbols(symbol="calculateTotal", path="src/")
Result: 23 references found across 8 files (src/billing, src/reports)
Status: Success
```

**Retry**:

```
【MCP Call Report】
Service: DuckDuckGo
Trigger: Search for recent Python 3.12 breaking changes
Parameters: search(query="Python 3.12 breaking changes", recencyDays=30)
Result: 429 Rate Limit → Retry with recencyDays=7 → 12 results from python.org
Status: Retry (429, narrowed time window)
```

**Fallback**:

```
【MCP Call Report】
Service: Context7 → DeepWiki
Trigger: Retrieve FastAPI middleware documentation
Parameters: resolve-library-id("FastAPI") → get-library-docs(topic="middleware")
Result: Context7 timeout → DeepWiki fallback → 3 wiki pages retrieved
Status: Fallback (timeout → DeepWiki)
```

**Empty Result**:

```
【MCP Call Report】
Service: DeepWiki
Trigger: Search for goroutine pool patterns in project wiki
Parameters: ask_question(repo="org/project", question="goroutine pool pattern")
Result: No matches found in wiki
Status: Success (empty result, requested user guidance)
```

### Reporting Discipline

- **Mandatory**: Report every MCP call, regardless of outcome
- **Concise**: Summarize parameters, don't dump full payloads
- **Actionable**: Include status to inform next steps
- **Transparent**: Document fallbacks and retries explicitly

---

## Development Principles

When applying these rules, adhere to core engineering principles:

- **KISS**: Choose simplest MCP server for the task; avoid over-engineering
- **YAGNI**: Don't call MCP servers speculatively; wait for concrete need
- **DRY**: Reuse Memory entries; avoid re-querying documentation
- **SOLID**: Each MCP call should have single, well-defined purpose

---
