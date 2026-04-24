# Universal Performance Optimization Plan

This document serves as a language-agnostic framework for optimizing software performance across frontend, backend, and infrastructure layers. It is designed to be used as a "Skill" or standard operating procedure for any high-performance project.

---

## 1. Phase One: The Diagnostic Audit
*Prioritize measurement over intuition.*

- [ ] **Establish Baselines**: Record current performance metrics (Initial Load, Time to Interactive, API Latency, Database Query Times).
- [ ] **Identify Bottlenecks**: Use profiling tools (Chrome DevTools, Flame Graphs, APM solutions) to find the "Top 3" slowest paths.
- [ ] **Payload Analysis**: Audit total data transfer. Identify large assets, uncompressed responses, or redundant data fields.

## 2. Phase Two: Frontend & Visual Performance
*Optimize the Critical Rendering Path.*

- [ ] **Asset Minification**: Ensure all JS, CSS, and HTML are minified and stripped of comments/dead code.
- [ ] **Image Optimization**:
    - Convert to modern formats (WebP/AVIF).
    - Implement Lazy Loading for off-screen media.
    - Use responsive image sets (`srcset`).
- [ ] **Critical CSS**: Inline styles required for "Above the Fold" content to prevent Render-Blocking.
- [ ] **Resource Prioritization**: Use `rel="preload"` for fonts and `rel="preconnect"` for critical third-party domains.

## 3. Phase Three: Logic & Computation
*Efficient execution and memory management.*

- [ ] **Algorithm Efficiency**: Replace O(n²) or O(n!) operations with O(n) or O(log n) alternatives where possible.
- [ ] **Memoization**: Cache the results of expensive function calls based on input arguments.
- [ ] **Concurrency**: Offload heavy computations to background threads (e.g., Web Workers) to keep the UI/Main Thread responsive.
- [ ] **Debouncing & Throttling**: Limit the execution frequency of high-rate events (scroll, resize, search input).

## 4. Phase Four: Data & Backend Strategy
*Reduce latency and payload size.*

- [ ] **Data Pagination**: Never fetch "all" records; implement cursor-based or offset pagination.
- [ ] **Selective Retrieval**: Request only the necessary fields (e.g., GraphQL or specific JSON keys) to reduce serialisation overhead.
- [ ] **Caching Strategy**:
    - **Client-side**: Use IndexedDB or LocalStorage for persistent data.
    - **Server-side**: Implement Redis or Memcached for frequent queries.
    - **HTTP**: Set proper `Cache-Control` headers (ETags, Max-Age).

## 5. Phase Five: Infrastructure & Networking
*Bring the data closer to the user.*

- [ ] **CDN Integration**: Serve static assets and media via Edge nodes.
- [ ] **Compression**: Enable Brotli or Gzip at the server/proxy level.
- [ ] **Connection Pooling**: Reuse database and network connections to avoid "TCP Handshake" overhead.
- [ ] **DNS Optimization**: Reduce the number of third-party domains to minimize DNS lookups.

---

## 6. Critical Performance Killers & Solutions
*Common patterns that destroy performance.*

### 🚀 N+1 Queries (Database/API)
- **The Problem**: Executing a separate database or API call for every item in a list (e.g., fetching 50 users, then 50 separate calls for their posts).
- **The Fix**: **Eager Loading**. Use `JOIN` clauses or batch requests (`IN [id1, id2...]`) to fetch all related data in a single round-trip.

### 🐢 Layout Thrashing (UI/Frontend)
- **The Problem**: Interleaving "Reads" (e.g., `offsetWidth`) and "Writes" (e.g., `style.width`) to the DOM. This forces the browser to re-calculate layout multiple times per frame.
- **The Fix**: **Batching**. Read all necessary values first, then perform all writes at once.

### 💾 Memory Leaks (Language-Agnostic)
- **The Problem**: Retaining references to objects that are no longer needed, preventing Garbage Collection (GC).
- **The Fix**: Clear timeouts/intervals, unregister event listeners, and set unused large objects/arrays to `null`.

### 📉 Unindexed Scans (Storage)
- **The Problem**: Querying a table without an index on the filtered columns, forcing a full table scan.
- **The Fix**: Ensure all columns used in `WHERE`, `JOIN`, or `ORDER BY` clauses are indexed.

---

## 7. Pro-Tip: How to Use This Skill Effectively
> [!IMPORTANT]
> **The "Trace First" Rule**: Never optimize code you haven't traced. You might spend hours optimizing a function that only accounts for 0.1% of total execution time. Trace the request from UI to DB to see exactly where the milliseconds are lost.

- **Avoid the Micro-benchmarking Trap**: Don't waste time on `for` vs `map` or `string` vs `concat` unless you are operating on millions of items. Focus on I/O, Network Latency, and DOM Manipulation first.
- **The 80/20 Rule**: 80% of performance gains usually come from 20% of the effort—specifically asset optimization and query efficiency.

---

## 8. Implementation Checklist (The "Skill" Loop)
1. **PROFILING**: Run a performance trace *before* any change.
2. **ISOLATION**: Change exactly one parameter at a time.
3. **VALIDATION**: Re-run the trace to confirm the improvement.
4. **REGRESSION TESTING**: Ensure performance doesn't degrade in other areas.

---
*Created as a Universal Skill for high-performance software engineering.*
