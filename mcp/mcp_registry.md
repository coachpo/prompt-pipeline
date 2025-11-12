# MCP Server Reference

## Table of Contents

- [Overview](#overview)
- [Server Registry](#server-registry)
  - [Documentation & Knowledge](#documentation--knowledge)
  - [Task Management & Planning](#task-management--planning)
  - [Browser Automation & Testing](#browser-automation--testing)
  - [System Operations](#system-operations)
  - [Design & UI Development](#design--ui-development)
- [Server Entry Template](#server-entry-template)

---

## Overview

This document catalogs Model Context Protocol (MCP) servers, providing technical specifications, capabilities, and integration guidelines. Each server entry includes repository links, API surface area, and usage recommendations for AI-assisted development workflows.

**Navigation Guidelines:**
- Servers are organized by functional category for scalability
- Use the priority matrix when multiple servers overlap in functionality
- Reference the entry template when documenting new servers

---

## Server Registry

### Documentation & Knowledge

#### Context7

**Repository:** https://github.com/upstash/context7

**Core Capabilities:**
- Version-specific documentation retrieval from official sources
- Library API resolution and disambiguation
- Diff-based changelog analysis between versions

**Use Cases:**
- Verifying API signatures and breaking changes
- Resolving deprecated/removed functionality
- Version upgrade planning and migration guides

**Priority Matrix:**
- vs **DeepWiki**: Official API reference → **Context7**; community practices/patterns → **DeepWiki**
- vs **DuckDuckGo**: Canonical documentation → **Context7**; recent discussions/blog posts → **DuckDuckGo**

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `resolve-library-id(libraryName)` | Resolve library to canonical Context7 ID | Disambiguate package names |
| `get-library-docs(id, tokens?, topic?)` | Fetch documentation segments by topic | Targeted API lookup and version comparison |

---

#### DeepWiki

**Repository:** https://cognition.ai/blog/deepwiki-mcp-server (Official Cognition AI)

**Core Capabilities:**
- GitHub repository wiki aggregation and indexing
- Project-specific best practices and contribution guidelines
- Semantic search across wiki content

**Use Cases:**
- Engineering patterns and architectural decisions
- Release processes and deployment procedures
- Onboarding documentation and code conventions

**Priority Matrix:**
- vs **Context7**: No official docs available OR need practical examples → **DeepWiki**; formal API reference → **Context7**
- vs **DuckDuckGo**: Project-specific knowledge → **DeepWiki**; external discussions/recent news → **DuckDuckGo**

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `read_wiki_structure(repo)` | Enumerate wiki table of contents | Navigation and topic discovery |
| `read_wiki_contents(repo)` | Retrieve full wiki content | Offline analysis and search |
| `ask_question(repo, question)` | Semantic Q&A over wiki content | Direct knowledge extraction |

---

#### DuckDuckGo Search

**Repository:** https://github.com/nickclyde/duckduckgo-mcp-server

**Core Capabilities:**
- Web search with temporal and domain-based filtering
- Content extraction and result ranking
- Privacy-focused search without tracking

**Use Cases:**
- Recent issue reports and bug discussions
- Release announcements and breaking changes
- Community blog posts and tutorials

**Priority Matrix:**
- vs **Context7/DeepWiki**: No canonical documentation → **DuckDuckGo** as fallback
- Supplement official docs with real-world usage examples and troubleshooting

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `search(query, recencyDays?, domains?)` | Execute search with filters | Time-bounded queries, domain whitelisting |

---

### Task Management & Planning

#### Sequential Thinking

**Repository:** https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking

**Core Capabilities:**
- Multi-step problem decomposition with revision support
- Hypothesis generation and verification workflows
- Dynamic planning with backtracking and branch exploration

**Use Cases:**
- Architecture design and refactoring planning
- Performance optimization strategies
- Cross-system integration design

**Priority Matrix:**
- vs **MCP Shrimp**: Planning and decomposition → **Sequential Thinking**; task tracking and verification → **MCP Shrimp**
- vs **All Execution Servers**: Generate plan → **Sequential Thinking**; execute steps → domain-specific servers

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `sequentialthinking(thought, thoughtNumber, totalThoughts, nextThoughtNeeded, isRevision?, revisesThought?, branchFromThought?, branchId?, needsMoreThoughts?)` | Structured thinking process with revision | Complex problem solving, planning |

---

#### MCP Shrimp Task Manager

**Repository:** https://github.com/cjo4m06/mcp-shrimp-task-manager

**Core Capabilities:**
- Natural language to structured task conversion
- Dependency tracking and subtask decomposition
- Task verification with scoring and quality gates
- Execution history and knowledge base accumulation

**Use Cases:**
- Feature development workflow management
- Sprint planning and task breakdown
- Quality assurance and acceptance testing
- Post-mortem analysis and retrospectives

**Priority Matrix:**
- vs **Sequential Thinking**: Planning phase → **Sequential Thinking** first; task execution and tracking → **MCP Shrimp**
- vs **Memory**: Task-specific knowledge → **MCP Shrimp**; long-term decisions/patterns → **Memory**

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `init_project_rules()` | Initialize project standards and guidelines | Project setup |
| `plan_task(description, requirements?)` | Generate actionable task plan from requirements | Task creation |
| `split_tasks(tasksRaw, updateMode, ...)` | Decompose tasks into subtasks | Task breakdown |
| `list_tasks(status)` | Query tasks by status | Progress tracking |
| `get_task_detail(id)` | Retrieve task details | Detailed inspection |
| `analyze_task(initialConcept, summary, ...)` | Technical complexity analysis | Risk assessment |
| `execute_task(id)` | Generate execution guidance | Implementation phase |
| `update_task(id, ...)` | Modify task properties | Iteration and refinement |
| `verify_task(id, score, summary)` | Quality gate verification | Acceptance testing |
| `delete_task(id)` / `clear_all_tasks(confirm)` | Task lifecycle management | Cleanup operations |

---

#### Memory

**Repository:** https://github.com/modelcontextprotocol/servers/tree/main/src/memory (Official MCP Knowledge Graph)

**Core Capabilities:**
- Persistent knowledge graph with entities, relations, and observations
- Semantic search and node traversal
- Causal chain visualization and reasoning
- Long-term context retention across sessions

**Use Cases:**
- Decision rationale and architectural decision records (ADRs)
- Incident post-mortems and root cause analysis
- Team knowledge base and tribal knowledge capture
- Historical context for recurring issues

**Priority Matrix:**
- vs **All Servers**: Post-execution knowledge persistence → **Memory**
- Query historical context before new implementations

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `create_entities(entities[])` | Define knowledge nodes | Entity modeling |
| `add_observations(...)` | Record facts and observations | Event logging |
| `create_relations(...)` | Establish entity relationships | Knowledge graph construction |
| `search_nodes(query)` | Semantic node search | Knowledge retrieval |
| `open_nodes(names[])` | Retrieve node details | Deep inspection |
| `read_graph()` | Export full graph | Analysis and visualization |
| `delete_entities(names[])` / `delete_observations(...)` / `delete_relations(...)` | Graph maintenance | Knowledge curation |

---

### Browser Automation & Testing

#### Playwright MCP

**Repository (Official):** https://github.com/microsoft/playwright-mcp  
**Repository (Community):** https://github.com/executeautomation/mcp-playwright

**Core Capabilities:**
- Cross-browser automation (Chromium, Firefox, WebKit)
- Accessibility-tree based element interaction
- Screenshot and network monitoring
- Multi-tab session management

**Use Cases:**
- End-to-end test generation and maintenance
- Form automation and workflow testing
- Regression testing with visual comparison
- Browser console and network debugging

**Priority Matrix:**
- vs **Chrome DevTools MCP**: Portable automation and testing → **Playwright**; deep debugging and performance profiling → **Chrome DevTools**
- vs **Desktop Commander**: Web-based automation → **Playwright**; filesystem/native apps → **Desktop Commander**

**API Surface:**

**Core Automation (19 tools):**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `browser_navigate(url)` | Navigate to URL | Test initialization |
| `browser_navigate_back()` | Go back to previous page | Navigation history |
| `browser_snapshot()` | Capture accessibility tree snapshot | Element inspection |
| `browser_click(element, ref, button?, doubleClick?, modifiers?)` | Click interaction | User action simulation |
| `browser_type(element, ref, text, slowly?, submit?)` | Text input | Form filling |
| `browser_fill_form(fields[])` | Multi-field form completion | Batch form operations |
| `browser_select_option(element, ref, values[])` | Select dropdown option | Form controls |
| `browser_hover(element, ref)` | Hover over element | Tooltip/menu triggers |
| `browser_drag(startElement, startRef, endElement, endRef)` | Drag and drop | Complex interactions |
| `browser_file_upload(paths[]?)` | Upload files | File input handling |
| `browser_handle_dialog(accept, promptText?)` | Handle alerts/prompts | Dialog management |
| `browser_press_key(key)` | Press keyboard key | Keyboard navigation |
| `browser_evaluate(function, element?, ref?)` | Execute JavaScript | Custom automation |
| `browser_wait_for(text?, textGone?, time?)` | Wait for conditions | Async UI stabilization |
| `browser_resize(width, height)` | Resize browser window | Viewport testing |
| `browser_take_screenshot(type?, filename?, element?, ref?, fullPage?)` | Capture screenshots | Visual testing |
| `browser_console_messages(onlyErrors?)` | Retrieve console logs | Error detection |
| `browser_network_requests()` | Capture network activity | API monitoring |
| `browser_close()` | Close browser page | Cleanup |

**Tab Management (1 tool):**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `browser_tabs(action, index?)` | List/create/close/select tabs | Multi-page workflows |

**Browser Installation (1 tool):**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `browser_install()` | Install browser executable | Environment setup |

**Coordinate-based/Vision (3 tools, opt-in via --caps=vision):**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `browser_mouse_click_xy(element, x, y)` | Click at coordinates | Pixel-based interaction |
| `browser_mouse_move_xy(element, x, y)` | Move mouse to coordinates | Hover positioning |
| `browser_mouse_drag_xy(element, startX, startY, endX, endY)` | Drag between coordinates | Canvas/drawing apps |

**PDF Generation (1 tool, opt-in via --caps=pdf):**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `browser_pdf_save(filename?)` | Save page as PDF | Document generation |

**Test Assertions (5 tools, opt-in via --caps=testing):**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `browser_verify_element_visible(role, accessibleName)` | Verify element visibility | Test assertions |
| `browser_verify_text_visible(text)` | Verify text presence | Content validation |
| `browser_verify_list_visible(element, ref, items[])` | Verify list contents | Collection assertions |
| `browser_verify_value(type, element, ref, value)` | Verify input values | Form validation |
| `browser_generate_locator(element, ref)` | Generate test locator | Test authoring |

**Tracing (2 tools, opt-in via --caps=tracing):**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `browser_start_tracing()` | Start trace recording | Performance analysis |
| `browser_stop_tracing()` | Stop trace recording | Trace capture |

---

#### Chrome DevTools MCP

**Repository:** https://github.com/ChromeDevTools/chrome-devtools-mcp

**Core Capabilities:**
- Browser automation with accessibility-tree based snapshots
- Performance profiling with Core Web Vitals analysis
- Network request monitoring and debugging
- DevTools integration for real-time inspection
- Multi-page/tab session management
- Device emulation and responsive testing

**Use Cases:**
- Performance bottleneck identification with trace analysis
- Automated web testing and user flow validation
- Network debugging and API inspection
- Responsive design verification across viewports
- Production debugging with console logs and network requests

**Priority Matrix:**
- vs **Playwright MCP**: DevTools integration and performance profiling → **Chrome DevTools**; cross-browser testing → **Playwright**
- vs **Documentation Servers**: Gather runtime diagnostics first → **Chrome DevTools**; then consult docs

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| **Input Automation (8 tools)** |
| `click(uid, dblClick?)` | Click on page elements | User interaction simulation |
| `fill(uid, value)` | Fill form fields | Form automation |
| `fill_form(elements[])` | Fill multiple fields at once | Batch form operations |
| `hover(uid)` / `drag(from_uid, to_uid)` | Mouse interactions | Complex UI testing |
| `press_key(key)` / `upload_file(uid, filePath)` | Keyboard and file inputs | Special interactions |
| **Navigation Automation (6 tools)** |
| `navigate_page(url, type?, ignoreCache?)` | Navigate to URL or reload | Page control |
| `new_page(url)` / `close_page(pageIdx)` | Manage tabs | Multi-page workflows |
| `list_pages()` / `select_page(pageIdx)` | Query and switch pages | Session management |
| `wait_for(text, timeout?)` | Wait for content | Synchronization |
| **Emulation (2 tools)** |
| `emulate(networkConditions?, cpuThrottlingRate?)` | Throttle network/CPU | Performance testing |
| `resize_page(width, height)` | Adjust viewport | Responsive testing |
| **Performance (3 tools)** |
| `performance_start_trace(reload, autoStop)` | Start recording | Performance profiling |
| `performance_stop_trace()` | Stop recording | Finalize trace |
| `performance_analyze_insight(insightSetId, insightName)` | Get detailed metrics | Diagnostic analysis |
| **Network (2 tools)** |
| `list_network_requests(resourceTypes?, pageSize?, includePreservedRequests?)` | List all requests | Network overview |
| `get_network_request(reqid?)` | Get request details | Deep inspection |
| **Debugging (5 tools)** |
| `take_snapshot(verbose?, filePath?)` | Capture accessibility tree | Element inspection |
| `take_screenshot(uid?, fullPage?, format?, quality?, filePath?)` | Visual capture | Visual testing |
| `evaluate_script(function, args?)` | Execute JavaScript | Runtime inspection |
| `list_console_messages(types?, pageSize?, includePreservedMessages?)` | List console logs | Error detection |
| `get_console_message(msgid)` | Get message details | Error analysis |

---

### System Operations

#### Desktop Commander

**Repository:** https://github.com/wonderwhy-er/DesktopCommanderMCP

**Core Capabilities:**
- Filesystem operations (CRUD, search, metadata)
- Process management and long-running command execution
- Cross-platform shell command support
- Audit logging and operation history

**Use Cases:**
- Batch file processing and organization
- Log collection and analysis
- Development environment setup
- System diagnostics and troubleshooting

**Priority Matrix:**
- vs **Local IDE/LSP tooling**: Use IDE/LSP features for semantic code navigation; use **Desktop Commander** for filesystem and process automation
- vs **Playwright**: Native filesystem/process → **Desktop Commander**; web automation → **Playwright**

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `create_directory(path)` | Create directory | Environment setup |
| `write_file(path, content)` | Write file content | File generation |
| `edit_block(path, start, end, replacement)` | Targeted file editing | Surgical edits |
| `move_file(source, destination)` | Move/rename files | Organization |
| `read_file(path)` / `read_multiple_files(paths[])` | Read file content | Inspection |
| `get_file_info(path)` / `list_directory(path)` | Metadata and listing | Navigation |
| `start_process(command, args[])` | Launch process | Command execution |
| `interact_with_process(pid, input)` | Process I/O | Interactive commands |
| `read_process_output(pid)` / `kill_process(pid)` | Process management | Lifecycle control |
| `start_search(query)` / `get_more_search_results` / `stop_search` | Filesystem search | Content discovery |
| `get_config` / `set_config_value` | Configuration management | Server settings |
| `get_recent_tool_calls` / `get_usage_stats` | Observability | Usage analytics |

---

#### MCP Server Time

**Repository:** https://github.com/modelcontextprotocol/servers/tree/main/src/time

**Core Capabilities:**
- Real-time clock queries for any IANA timezone with DST awareness
- Lossless timezone conversion and validation between arbitrary regions
- Deterministic "local" timezone override via `--local-timezone` for consistent scheduling defaults

**Use Cases:**
- Coordinating deploy/maintenance windows across distributed teams
- Generating compliance-friendly timestamps for payloads, audit trails, or announcements
- Verifying stakeholder-supplied meeting times before creating plans or tasks

**Priority Matrix:**
- vs **Desktop Commander**: Use **Desktop Commander** for filesystem/process automation; use **MCP Server Time** strictly for temporal intelligence
- vs **Documentation & Knowledge Servers**: Use MCP Server Time when you already know the policy and just need authoritative timestamps; use documentation servers when researching the policy itself

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `get_current_time(timezone?)` | Return the current timestamp for a requested timezone or the configured default | Confirm "now" before scheduling deployments or status updates |
| `convert_time(source_timezone, time, target_timezone)` | Translate a wall-clock time between regions | Validate that handoff/checkpoint times align across teams |

**Configuration Notes:**

```json
"mcp-server-time": {
  "command": "uvx",
  "args": ["mcp-server-time", "--local-timezone=Asia/Shanghai"]
}
```

Set `--local-timezone` to ensure the server defaults to the stakeholder’s region when no timezone argument is passed.

---

### Design & UI Development

#### Figma

**Repository:** https://github.com/GLips/Figma-Context-MCP

**Core Capabilities:**
- Figma file structure parsing and node traversal
- Design token extraction (colors, typography, spacing)
- Asset export (SVG, PNG) with cropping support
- Component and annotation retrieval

**Use Cases:**
- Design system implementation and validation
- Icon and asset pipeline automation
- Design-to-code handoff verification
- Design constraint extraction for developers

**Priority Matrix:**
- vs **shadcn**: Design extraction → **Figma**; component implementation → **shadcn**

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `get_figma_data(fileKey, depth?, nodeId?)` | Fetch file structure and nodes | Design inspection |
| `download_figma_images(fileKey, localPath, nodes[], pngScale?)` | Export images | Asset pipeline |

---

#### shadcn

**Repository:** https://github.com/Jpisnice/shadcn-ui-mcp-server

**Core Capabilities:**
- shadcn/ui component registry integration
- Component source code and documentation retrieval
- Usage example and demo fetching
- Installation command generation

**Use Cases:**
- Component selection and evaluation
- UI scaffolding and prototyping
- Design system consistency
- Framework-specific implementation (React, Svelte, Vue)

**Priority Matrix:**
- vs **Figma**: Design tokens/assets → **Figma** first; component implementation → **shadcn**

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `get_project_registries()` | List configured registries | Registry discovery |
| `list_items_in_registries(registries[], limit?, offset?)` | Browse components | Component catalog |
| `search_items_in_registries(query, registries[], limit?, offset?)` | Fuzzy search | Component discovery |
| `view_items_in_registries(items[])` | Retrieve detailed specs | Component inspection |
| `get_item_examples_from_registries(query, registries[])` | Fetch usage examples | Implementation reference |
| `get_add_command_for_items(items[])` | Generate install command | Project integration |
| `get_audit_checklist()` | Pre-deployment validation | Quality assurance |

---

## Server Entry Template

Use this template when adding new MCP servers to maintain consistency:

```markdown
#### Server Name

**Repository:** `https://github.com/org/repo`

**Core Capabilities:**
- Key capability 1
- Key capability 2
- Key capability 3

**Use Cases:**
- Specific scenario 1
- Specific scenario 2
- Specific scenario 3

**Priority Matrix:**
- vs **Server X**: When to use this server vs Server X
- vs **Server Y**: When to use this server vs Server Y

**API Surface:**

| Function | Description | Usage Pattern |
|----------|-------------|---------------|
| `function_name(params)` | What it does | When to use it |
| `another_function(params)` | What it does | When to use it |
```

**Documentation Standards:**
- **Repository**: Must link to official or most authoritative implementation
- **Core Capabilities**: Technical features, not business outcomes
- **Use Cases**: Concrete scenarios with verifiable outcomes
- **Priority Matrix**: Decision criteria when multiple servers overlap
- **API Surface**: Table format for scannability; include parameter signatures
