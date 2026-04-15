

# Advanced Database Indexing: Senior Backend Engineer Revision Guide

This guide is structured for a backend engineer with 5+ years of experience preparing for system design and advanced database interview rounds. It focuses heavily on real-world application, internal mechanics, and PostgreSQL-specific optimizations.

---

## 1. Core Mechanics & The B-Tree

### What is an Index?
An index is a separate data structure that stores a subset of table columns and a pointer to the physical row on disk. It transforms an **O(N) Full Table Scan** into an **O(log N) Index Seek**.

### The B-Tree (Balanced Tree)
The default index structure in almost all relational databases.
* **Structure:** Root Node -> Branch Nodes -> Leaf Nodes.
* **Leaf Nodes:** Contain the actual indexed value and the physical disk pointer (or the primary key in a non-clustered index). In a clustered index, the leaf node *is* the data page.
* **Best for:** Exact matches (`=`), ranges (`>`, `<`, `BETWEEN`), and sorting (`ORDER BY`).

---

## 2. Advanced Indexing Strategies

### The Cost of Indexing
* **Read vs. Write Trade-off:** Every `INSERT`, `UPDATE`, or `DELETE` requires the database to update the table *and* rebalance the B-Tree. Over-indexing cripples write performance.

### Composite Indexes & The Left-Most Prefix Rule
Indexing multiple columns together: `CREATE INDEX idx_user_status ON users (tenant_id, status);`
* **The Rule:** The index can only be used if the query filters on the columns from left to right.
* *Valid usage:* `WHERE tenant_id = 5 AND status = 'active'`
* *Valid usage:* `WHERE tenant_id = 5`
* *Invalid usage (Index bypass):* `WHERE status = 'active'` (The DB doesn't know which branch to start traversing).

### Covering Indexes (Index-Only Scans)
When the `SELECT` clause only requests columns that are already present in the index.
* **Benefit:** The database fetches the data directly from the B-Tree's leaf nodes and completely skips the expensive I/O trip to the main table on the hard drive.
* *Example:* `SELECT status FROM users WHERE tenant_id = 5;` (Using the composite index above).

### Partial Indexes
Indexing only a subset of data using a `WHERE` clause in the index definition.
* *Example:* `CREATE INDEX idx_active_users ON users (last_login) WHERE status = 'active';`
* **Benefit:** Drastically saves disk space and memory, and keeps write speeds high because the index is only updated when an 'active' user is modified.

---

## 3. Physical Storage & Specialized Indexes (PostgreSQL)

### Clustered vs. Non-Clustered
* **Clustered (Primary Key):** Dictates the physical sort order of the data on disk. You can only have ONE per table. Searching it is the fastest possible operation.
* **Non-Clustered:** A separate structure that points back to the clustered index or physical disk address. You can have many.

### Specialized Indexes (Beyond the B-Tree)
As backend systems integrate more complex data structures (like AI metadata or spatial features), B-Trees fall short.
* **Hash Index:** Purely for `O(1)` exact matches (`=`). Cannot do ranges. Smaller footprint than B-Tree.
* **GiST (Generalized Search Tree):** Used for multidimensional/geometric data (e.g., PostGIS spatial searches, "Find locations within 5km").
* **GIN (Generalized Inverted Index):** Crucial for indexing **JSONB** payloads or Full-Text Search. If you are storing AI-generated unstructured metadata or configurations in a `JSONB` column, a GIN index allows you to query individual keys inside the JSON instantly.

### Index Maintenance
* **Fragmentation:** Frequent writes cause index pages to split and scatter.
* **Fix:** Commands like `VACUUM ANALYZE` or `REINDEX` (in PostgreSQL) rebuild the tree physically, reclaiming space and restoring query performance.

---

## 4. Scenario-Based Interview Questions (Senior Level)

### Scenario 1: The Write-Heavy System
**Question:** *"We have a `logs` table processing 10,000 inserts per second. We need to occasionally query error logs by `error_code` and `timestamp`. A junior dev suggests indexing both columns. What is your architectural review?"*

**Answer:** Indexing a high-throughput insert table is dangerous because of the write penalty and constant B-Tree rebalancing. Instead of a standard composite index, I would implement a **Partial Index** on `(timestamp)` where `level = 'ERROR'`. Since 'ERROR' logs likely make up a tiny fraction of the total 10k/sec volume, this keeps the index incredibly small, minimizes the write penalty on standard INFO logs, but makes the exact query we need lightning fast. Additionally, I'd suggest partitioning the table by date to drop old logs efficiently.

### Scenario 2: The Multi-Tenant SaaS
**Question:** *"You have a multi-tenant PostgreSQL database with a `users` table of 50 million rows. You frequently run `SELECT name, email FROM users WHERE company_id = ? AND role = 'ADMIN'`. How do you optimize this?"*

**Answer:** I would create a composite index. The order matters: `(company_id, role)`. Because `company_id` has high cardinality (many unique values) and `role` has low cardinality, `company_id` goes first to prune the tree fastest (Left-Most Prefix). 
To take it a step further to Senior-level optimization, I would utilize a **Covering Index** (using the `INCLUDE` clause in Postgres): `CREATE INDEX idx_company_admin ON users (company_id, role) INCLUDE (name, email);`. This allows for an Index-Only Scan, retrieving the name and email directly from the index without touching the table's heap storage.

### Scenario 3: AI Integration & Unstructured Data
**Question:** *"We are integrating an LLM that processes user documents and outputs a varying set of extracted entities (e.g., {"sentiment": "positive", "entities": ["apple", "tech"], "confidence": 0.98}). We store this in a JSONB column called `ai_metadata`. How do you ensure we can query users based on specific entities instantly without scanning the whole table?"*

**Answer:** A standard B-Tree cannot index the internal keys of a JSON object effectively. I would create a **GIN (Generalized Inverted Index)** on the `ai_metadata` column. Specifically, a `jsonb_ops` or `jsonb_path_ops` GIN index. This cracks open the JSON payload and indexes the individual key-value pairs, allowing queries like `WHERE ai_metadata @> '{"entities": ["apple"]}'` to execute with an index seek rather than a full sequential scan of the JSON text.

### Scenario 4: Query Debugging
**Question:** *"You added an index, but the query is still slow. How do you debug this?"*

**Answer:** I would prefix the query with `EXPLAIN ANALYZE`. This executes the query and returns the database's execution plan. I would look for:
1.  **Seq Scan vs. Index Scan:** Is it ignoring the index entirely? If so, the statistics might be outdated, requiring a `VACUUM ANALYZE`. Or, the query might be fetching a massive percentage of the table, in which case the Query Planner calculates that a sequential scan is actually faster than jumping back and forth between the index and the table.
2.  **Filter conditions:** Is there a function wrapping the indexed column? (e.g., `WHERE LOWER(email) = 'test'`). This breaks standard indexes. I would need to replace it with a Function-Based Index.
database-indexing-revision-guide.md
Displaying database-indexing-revision-guide.md.
