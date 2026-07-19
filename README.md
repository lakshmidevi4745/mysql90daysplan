# mysql90daysplan

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>90-Day SQL & BigQuery Mastery Tracker</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700&family=Syne:wght@600;700;800&display=swap');

* { margin:0; padding:0; box-sizing:border-box; }

:root{
  --bg:#0a0a0f;
  --panel:#12121a;
  --panel2:#181822;
  --border:#26262f;
  --text:#e8e8ee;
  --dim:#8a8a98;
  --accent:#7c5cff;
  --accent2:#00e5b0;
  --warn:#ffb020;
  --done:#00e5b0;
}

body{
  background:var(--bg);
  color:var(--text);
  font-family:'JetBrains Mono', monospace;
  min-height:100vh;
}

.wrap{ max-width:1000px; margin:0 auto; padding:20px 16px 80px; }

header{ margin-bottom:20px; }
h1{
  font-family:'Syne', sans-serif;
  font-weight:800;
  font-size:clamp(22px,5vw,32px);
  letter-spacing:-0.5px;
  background:linear-gradient(90deg,#fff,var(--accent2));
  -webkit-background-clip:text;
  background-clip:text;
  color:transparent;
}
.sub{ color:var(--dim); font-size:13px; margin-top:6px; }

.stats{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:10px;
  margin:18px 0;
}
.stat{
  background:var(--panel);
  border:1px solid var(--border);
  border-radius:10px;
  padding:12px;
  text-align:center;
}
.stat .num{ font-family:'Syne',sans-serif; font-size:24px; font-weight:700; color:var(--accent2); }
.stat .lbl{ font-size:10px; color:var(--dim); text-transform:uppercase; letter-spacing:1px; margin-top:2px; }

.progress-bar{
  height:8px;
  background:var(--panel2);
  border-radius:5px;
  overflow:hidden;
  margin-top:14px;
  border:1px solid var(--border);
}
.progress-fill{
  height:100%;
  background:linear-gradient(90deg,var(--accent),var(--accent2));
  transition:width .4s ease;
  border-radius:5px;
}

.tabs{
  display:flex;
  gap:6px;
  overflow-x:auto;
  padding:4px 0 12px;
  margin-bottom:6px;
  border-bottom:1px solid var(--border);
}
.tab{
  flex:none;
  padding:7px 12px;
  font-size:11px;
  font-weight:600;
  border-radius:7px;
  background:var(--panel);
  border:1px solid var(--border);
  color:var(--dim);
  cursor:pointer;
  white-space:nowrap;
  transition:.15s;
}
.tab:hover{ color:var(--text); border-color:var(--accent); }
.tab.active{ background:var(--accent); color:#fff; border-color:var(--accent); }

.day-list{ display:flex; flex-direction:column; gap:8px; margin-top:10px; }

.day-card{
  background:var(--panel);
  border:1px solid var(--border);
  border-radius:10px;
  padding:12px 14px;
  cursor:pointer;
  transition:.15s;
}
.day-card:hover{ border-color:var(--accent); }
.day-card.done{ border-color:var(--done); background:linear-gradient(180deg,rgba(0,229,176,0.06),transparent); }
.day-card.review{ border-left:3px solid var(--warn); }

.day-head{ display:flex; align-items:center; gap:10px; }
.checkbox{
  width:20px; height:20px; flex:none;
  border:2px solid var(--border);
  border-radius:6px;
  display:flex; align-items:center; justify-content:center;
  transition:.15s;
}
.day-card.done .checkbox{ background:var(--done); border-color:var(--done); }
.checkbox svg{ width:12px; height:12px; opacity:0; transition:.15s; }
.day-card.done .checkbox svg{ opacity:1; }

.day-num{
  font-family:'Syne',sans-serif;
  font-weight:700;
  font-size:13px;
  color:var(--accent2);
  flex:none;
  width:44px;
}
.day-topic{ font-size:13px; font-weight:500; flex:1; line-height:1.4; }
.day-card.done .day-topic{ color:var(--dim); text-decoration:line-through; text-decoration-color:var(--border); }

.chevron{ color:var(--dim); font-size:12px; transition:.2s; flex:none; }
.day-card.open .chevron{ transform:rotate(90deg); }

.day-detail{
  max-height:0;
  overflow:hidden;
  transition:max-height .25s ease;
}
.day-card.open .day-detail{ max-height:260px; margin-top:12px; }

.detail-block{
  background:var(--panel2);
  border-radius:8px;
  padding:10px 12px;
  margin-bottom:8px;
  font-size:12px;
  line-height:1.5;
  position:relative;
}
.detail-block .k{
  color:var(--accent);
  font-weight:700;
  font-size:10px;
  text-transform:uppercase;
  letter-spacing:1px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-bottom:4px;
}
.detail-block.practice .k{ color:var(--accent2); }
.detail-text{ padding-right:60px; display:block; }
.copy-btn{
  background:var(--panel);
  border:1px solid var(--border);
  color:var(--dim);
  font-family:'JetBrains Mono',monospace;
  font-size:9px;
  font-weight:700;
  text-transform:uppercase;
  letter-spacing:.5px;
  padding:3px 7px;
  border-radius:5px;
  cursor:pointer;
  transition:.15s;
  flex:none;
}
.copy-btn:hover{ color:var(--text); border-color:var(--accent); }
.copy-btn.copied{ color:var(--done); border-color:var(--done); }

.review-tag{
  display:inline-block;
  background:rgba(255,176,32,0.15);
  color:var(--warn);
  font-size:9px;
  font-weight:700;
  padding:2px 6px;
  border-radius:5px;
  margin-left:8px;
  text-transform:uppercase;
  letter-spacing:.5px;
}

footer{
  text-align:center;
  color:var(--dim);
  font-size:11px;
  margin-top:24px;
}

@media(max-width:480px){
  .day-num{ width:34px; font-size:12px; }
  .day-topic{ font-size:12px; }
}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <h1>105-Day SQL &amp; BigQuery Mastery</h1>
    <div class="sub">1hr learning + 1hr practice daily · comprehensive SQL, MySQL ops, BigQuery analytics · tap a day to expand · tap the box to mark done</div>
  </header>

  <div class="stats">
    <div class="stat"><div class="num" id="statDone">0</div><div class="lbl">Days Done</div></div>
    <div class="stat"><div class="num" id="statPct">0%</div><div class="lbl">Complete</div></div>
    <div class="stat"><div class="num" id="statPhase">1/6</div><div class="lbl">Current Phase</div></div>
  </div>
  <div class="progress-bar"><div class="progress-fill" id="progressFill" style="width:0%"></div></div>

  <div class="tabs" id="tabs"></div>
  <div class="day-list" id="dayList"></div>

  <footer>Comprehensive 105-day data engineering track · covers MySQL ops, SQL mastery, BigQuery analytics & architecture · progress auto-saves</footer>
</div>

<script>
const phases = [
  {id:1, name:"P1 · Foundations", range:[1,20]},
  {id:2, name:"P2 · Intermediate", range:[21,40]},
  {id:3, name:"P3 · DB Design", range:[41,60]},
  {id:4, name:"P4 · Advanced SQL", range:[61,75]},
  {id:5, name:"P5 · BigQuery Mastery", range:[76,95]},
  {id:6, name:"P6 · Capstone", range:[96,105]},
];

const days = [
{d:1,t:"RDBMS basics, tables/rows/columns, SQL vs NoSQL, SQL dialects (MySQL vs BigQuery architecture overview)",p:"Setup MySQL Local/Docker and BigQuery sandbox, navigate and explore data catalogs"},
{d:2,t:"SELECT, FROM, WHERE, comparison operators, NULL basics",p:"Write 15 basic SELECT/WHERE queries on sample dataset"},
{d:3,t:"AND, OR, NOT, IN, BETWEEN, LIKE, wildcards (%, _)",p:"Filtering exercises combining multiple conditions"},
{d:4,t:"ORDER BY, LIMIT/OFFSET, DISTINCT",p:"Pagination & top-N queries"},
{d:5,t:"Data types deep dive: INT, DECIMAL, VARCHAR, DATE, TIMESTAMP, BOOLEAN, casting",p:"Type conversion exercises, common casting pitfalls"},
{d:6,t:"Arithmetic & string functions: CONCAT, SUBSTRING, TRIM, UPPER/LOWER, LENGTH, REPLACE",p:"String manipulation drills"},
{d:7,t:"Date/time functions: DATE_ADD, DATEDIFF, EXTRACT, formatting, timezones",p:"Date arithmetic exercises"},
{d:8,t:"NULL deep dive: IS NULL, COALESCE, NULLIF, three-valued logic",p:"Queries handling missing data correctly"},
{d:9,t:"Aggregate functions: COUNT, SUM, AVG, MIN, MAX",p:"Summary stat queries"},
{d:10,t:"GROUP BY fundamentals, grouping rules, HAVING vs WHERE",p:"Grouped aggregation exercises"},
{d:11,t:"CASE WHEN expressions (simple + searched), conditional logic",p:"Build conditional buckets/flags in queries"},
{d:12,t:"Table creation: CREATE TABLE, constraints (PK, NOT NULL, UNIQUE, DEFAULT, CHECK)",p:"Design & create 3 related tables"},
{d:13,t:"INSERT, UPDATE, DELETE, TRUNCATE vs DELETE, MERGE/UPSERT intro",p:"DML practice with rollback scenarios"},
{d:14,t:"ALTER TABLE: add/drop/modify columns, renaming, DROP TABLE",p:"Schema evolution exercise"},
{d:15,t:"Keys: Primary, Foreign, Composite, Candidate, Surrogate",p:"Design a 3-table schema with proper keys"},
{d:16,t:"INNER JOIN deep dive: mechanics, syntax variations, join conditions",p:"10 inner join queries across multiple tables"},
{d:17,t:"LEFT/RIGHT JOIN, understanding NULLs from outer joins",p:"Outer join exercises with NULL handling"},
{d:18,t:"FULL OUTER JOIN, CROSS JOIN, SELF JOIN",p:"Self-join (hierarchy) + cross-join use case"},
{d:19,t:"Multi-table joins (3+ tables), join order & readability",p:"Complex multi-join query building"},
{d:20,t:"REVIEW: Recap Days 1-19, self-test with mixed questions",p:"Timed mock quiz (build your own 20 Qs, solve them)",r:true},

{d:21,t:"Subqueries: scalar, single-row, multi-row",p:"Write nested subqueries in WHERE clause"},
{d:22,t:"Correlated subqueries, EXISTS/NOT EXISTS",p:"Correlated subquery exercises"},
{d:23,t:"ANY, ALL, subqueries in SELECT/FROM (derived tables)",p:"Rewrite queries using derived tables"},
{d:24,t:"Common Table Expressions (CTEs): WITH clause basics",p:"Convert subqueries into CTEs"},
{d:25,t:"Multiple/chained CTEs, readability best practices",p:"Build a 3-step chained CTE pipeline"},
{d:26,t:"Recursive CTEs: syntax, use cases (hierarchies, sequences)",p:"Solve an org-chart/hierarchy recursive query"},
{d:27,t:"Window functions intro: OVER(), PARTITION BY, ORDER BY",p:"Basic window function queries"},
{d:28,t:"Ranking functions: ROW_NUMBER, RANK, DENSE_RANK, NTILE",p:"Top-N per group exercises"},
{d:29,t:"Aggregate window functions: running totals, moving averages",p:"Cumulative sum & rolling average queries"},
{d:30,t:"Offset window functions: LAG, LEAD, FIRST_VALUE, LAST_VALUE",p:"Period-over-period comparison queries"},
{d:31,t:"Window frames: ROWS BETWEEN, RANGE BETWEEN",p:"Custom frame window queries"},
{d:32,t:"Set operations: UNION, UNION ALL, INTERSECT, EXCEPT",p:"Combine datasets with set ops"},
{d:33,t:"PIVOT/UNPIVOT concepts (manual pivoting with CASE + aggregate)",p:"Build a pivot table manually"},
{d:34,t:"Views: CREATE VIEW, materialized views concept (MySQL vs BigQuery caching/refresh)",p:"Create 2 views for reporting use case"},
{d:35,t:"Stored procedures & functions basics (OLTP business logic vs BigQuery ELT scripting)",p:"Write a simple stored procedure/UDF in MySQL and BigQuery"},
{d:36,t:"Triggers: concept, use cases, pros/cons (Why Triggers don't exist in BigQuery)",p:"Read/trace a MySQL trigger example and design an alternative cloud architecture"},
{d:37,t:"Transactions: BEGIN, COMMIT, ROLLBACK, SAVEPOINT",p:"Transaction practice with rollback scenarios"},
{d:38,t:"ACID properties explained in depth with real examples (Row-level ACID vs BigQuery Batch mutations)",p:"Write notes/examples mapping ACID to real transactional vs analytical constraints"},
{d:39,t:"Isolation levels & MySQL Locks: Shared/Exclusive, Intent, Next-Key locks, Read Uncommitted to Serializable",p:"Conceptual quiz + diagram isolation levels and deadlock resolution"},
{d:40,t:"REVIEW: Mixed practice - joins + CTEs + window functions",p:"Solve 15 mixed-difficulty problems (LeetCode/StrataScratch style)",r:true},

{d:41,t:"ER Modeling: entities, attributes, relationships, cardinality",p:"Draw an ER diagram for an e-commerce system"},
{d:42,t:"Normalization: 1NF, 2NF, 3NF with examples",p:"Normalize a messy sample table to 3NF"},
{d:43,t:"BCNF, denormalization trade-offs (OLTP vs OLAP modeling rules)",p:"Denormalize a normalized schema for reporting use case"},
{d:44,t:"Indexing fundamentals & MySQL Storage: B-Tree structure, InnoDB Buffer Pool architecture and tuning",p:"Add indexes and analyze how the InnoDB Buffer Pool caches memory pages"},
{d:45,t:"Index types: clustered vs non-clustered, composite, covering indexes, index hints (USE/IGNORE/FORCE INDEX)",p:"Design indexes for query patterns, practice optimizer hints, analyze index performance"},
{d:46,t:"Query execution plans: EXPLAIN / EXPLAIN ANALYZE (MySQL vs BigQuery execution tree)",p:"Run EXPLAIN on 5 queries, interpret plans and identify bottlenecks"},
{d:47,t:"Query optimizer basics: cost-based optimization, database statistics, histogram collection, MySQL system metrics",p:"Compare optimized vs unoptimized query plans using EXPLAIN FORMAT=JSON"},
{d:48,t:"Common performance killers: SELECT *, functions on indexed cols, implicit conversions, query plan caching",p:"Rewrite 5 bad queries for performance, understand query plan caching benefits"},
{d:49,t:"Partitioning concepts: horizontal/vertical, sharding intro, MySQL Redo vs Undo vs Binlog architecture",p:"Conceptual design: partition a large MySQL table and review transaction logging paths"},
{d:50,t:"Concurrency control: locking (row/table/page), deadlocks, Next-Key lock phantom read prevention, MVCC basics",p:"Trace a deadlock scenario in MySQL and resolve it, understand MVCC snapshots"},
{d:51,t:"Storage engines overview (InnoDB row-store mechanics vs Columnar storage concepts), connection pooling",p:"Compare row-store vs column-store use cases for performance profiles, configure connection pool settings"},
{d:52,t:"Data integrity: referential integrity, cascading updates/deletes",p:"Implement cascade rules in schema"},
{d:53,t:"Data warehousing: OLTP vs OLAP, star schema, snowflake schema",p:"Design a star schema for sales data"},
{d:54,t:"Slowly Changing Dimensions (SCD Type 1/2/3), temporal tables, versioning strategies",p:"Model an SCD Type 2 dimension table with temporal versioning"},
{d:55,t:"Monitoring & Logging: slow query log, query logging, MySQL metrics, performance schema analysis",p:"Enable slow query log, analyze top slow queries, setup query profiling"},
{d:56,t:"Backup & Recovery strategies: mysqldump, binary backups, point-in-time recovery (PITR), disaster recovery",p:"Perform full backup/restore, setup PITR pipeline, practice recovery scenarios"},
{d:57,t:"Replication & High Availability: MySQL master-slave replication, failover mechanisms, read replicas, binary log formats",p:"Setup MySQL master-slave replication, practice failover and sync troubleshooting"},
{d:58,t:"Full-text search: FTS indexes in MySQL, relevance scoring, natural language search modes",p:"Create FTS index, build search queries with relevance ranking"},
{d:59,t:"REVIEW: Database administration & operational excellence",p:"Design fault-tolerant architecture: backup, replication, monitoring, failover automation",r:true},
{d:60,t:"REVIEW: Schema design, performance tuning, operational readiness mock",p:"Full case study: design production schema with HA, replication, backups, monitoring",r:true},

{d:61,t:"Advanced string functions: regex (REGEXP, RLIKE), pattern extraction",p:"Regex-based filtering/extraction queries"},
{d:62,t:"JSON in SQL: JSON_EXTRACT, parsing nested JSON columns (MySQL native JSON vs BigQuery JSON data types)",p:"Query nested JSON fields and optimize query access routes"},
{d:63,t:"Array/struct handling (esp. for BigQuery), INFORMATION_SCHEMA metadata navigation",p:"Query array columns, UNNEST basics, and write dynamic SQL using system metadata views"},
{d:64,t:"Advanced CASE + COALESCE patterns for data cleaning",p:"Build a data-cleaning query pipeline"},
{d:65,t:"Handling duplicates: detection, ROW_NUMBER() dedup pattern",p:"Deduplicate a messy dataset"},
{d:66,t:"Gaps and islands problem (sequences, streaks)",p:"Solve a gaps-and-islands SQL problem"},
{d:67,t:"Pivoting time-series data, cohort analysis basics",p:"Build a cohort retention query"},
{d:68,t:"Advanced joins: anti-joins, semi-joins, lateral joins",p:"Write anti-join/semi-join queries"},
{d:69,t:"Query refactoring: nested subqueries into readable CTEs",p:"Refactor 3 complex legacy queries"},
{d:70,t:"Performance tuning practice: indexing + query rewrite combined",p:"Tune 5 slow queries end-to-end"},
{d:71,t:"SQL for analytics: percentile functions, NTILE, statistical aggregates",p:"Compute percentiles/quartiles on sample data"},
{d:72,t:"Handling large datasets: batching, chunked processing concepts",p:"Simulate batch processing logic in SQL"},
{d:73,t:"SQL security basics: SQL injection, parameterized queries, least privilege",p:"Review/identify injection-vulnerable query patterns"},
{d:74,t:"SQL style & readability: formatting conventions, CTE-first style",p:"Refactor your own old queries to clean style"},
{d:75,t:"REVIEW: Timed mock interview - 15 advanced SQL problems",p:"Solve under time pressure, review mistakes",r:true},

{d:76,t:"BigQuery architecture deep dive: Dremel execution engine, Colossus storage, serverless model, slot allocations, Flex Slots",p:"Explore BigQuery console, run sample queries, analyze execution slots usage and Flex Slots pricing"},
{d:77,t:"Datasets, tables, table types (native, external, views, BigQuery Search Indexes, snapshots)",p:"Create dataset + native table, configure a Search Index for fast unstructured text lookup, snapshot table versions"},
{d:78,t:"Standard SQL vs Legacy SQL, BQ-specific syntax differences",p:"Convert a standard query to BQ dialect nuances"},
{d:79,t:"Partitioned tables: time-unit, ingestion-time, integer-range, partition pruning mechanics, partition expiration",p:"Create a partitioned table, query with partition filter, analyze bytes scanned, setup partition expiration policies"},
{d:80,t:"Clustering in BigQuery: how it works, + partitioning combined, BI Engine acceleration, cluster optimization",p:"Create clustered table, benchmark query cost before/after, monitor BI Engine reservations"},
{d:81,t:"Query cost model: bytes scanned, on-demand vs flat-rate/editions pricing, slot utilization, cost estimation",p:"Estimate query cost using dry-run capabilities, use cost analysis tools"},
{d:82,t:"Cost optimization: column pruning, partition pruning, caching, smart materialized view rewrites, result caching",p:"Optimize 5 queries to reduce bytes scanned, leverage result caching, automatic MV rewrites"},
{d:83,t:"Nested & repeated fields, STRUCT, ARRAY, UNNEST in depth, FLATTEN for nested data extraction",p:"Query deeply nested JSON-like BQ tables, flatten complex structures"},
{d:84,t:"BQ functions: analytic functions, APPROX_* functions, ARRAY_AGG, GIS/spatial functions (ST_DISTANCE, ST_CONTAINS)",p:"Practice BQ-specific aggregate/analytic functions and geospatial queries"},
{d:85,t:"Materialized views & scheduled queries in BigQuery, automatic refresh policies, view optimization",p:"Create a materialized view + scheduled query, optimize refresh frequency"},
{d:86,t:"BigQuery ML basics: CREATE MODEL, linear regression, time series forecasting, BQML model deployment",p:"Train a linear regression model and time series forecast on sample data"},
{d:87,t:"Data ingestion architectures: batch loads vs Storage Write API, streaming ingestion limits, error handling & retries",p:"Configure high-performance batch and streaming pipelines with error handling"},
{d:88,t:"BQ security: IAM roles, row-level security, authorized/column-level views, data masking, encryption",p:"Set up an authorized view with restricted columns and row-level access parameters"},
{d:89,t:"Change Data Capture (CDC) & Pipeline Integration: MySQL Binlog replication to BigQuery via GCP Datastream, dbt orchestration",p:"Sketch a pipeline: Operational MySQL to GCS/Datastream to BQ to dbt transform"},
{d:90,t:"BigQuery Omni & Federated Queries: query external data (Cloud SQL, Datastore, Cloud Storage), multi-cloud analytics",p:"Execute federated queries across BigQuery and Cloud SQL, Bigtable, understand cross-cloud data movement"},
{d:91,t:"Time Travel & Snapshots: query table at specific timestamp, snapshot management, CLONE for testing",p:"Practice querying historical table snapshots, clone and test schema changes safely"},
{d:92,t:"Data Catalog & Lineage: metadata management, data lineage tracking, governance policies, tagging",p:"Organize dataset taxonomy in Data Catalog, trace data lineage through transformation pipelines"},
{d:93,t:"BigQuery Dataflow integration: Apache Beam pipelines, autoscaling, windowed processing for streaming",p:"Design Beam pipeline for ETL, understand windowing for time-series aggregations"},
{d:94,t:"REVIEW: BigQuery advanced features & cost optimization deep dive",p:"Optimize complex ETL: ML model training, federated queries, cost vs performance trade-offs",r:true},
{d:95,t:"REVIEW: BigQuery mastery case study combining all features",p:"End-to-end BigQuery project: ingest, transform, optimize costs, deploy ML, setup CDC pipelines",r:true},

{d:96,t:"Capstone planning: design end-to-end scenario (MySQL production DB to BigQuery Data Warehouse), schema architecture + partition + cluster strategy",p:"Start capstone: MySQL source architecture design, Datastream landing zone, and target BQ schema with partitioning"},
{d:97,t:"Capstone build Phase 1: core analytical queries (joins, window fns, CTEs) and indexing configurations",p:"Build analytical query layer with proper MySQL indexes and BQ clustering, validate execution plans"},
{d:98,t:"Capstone build Phase 2: cost optimization + materialized views + ML feature engineering",p:"Implement materialized views, extract features for BQML, document cost optimization decisions"},
{d:99,t:"Capstone deployment & monitoring: setup CDC pipeline, monitoring alerts, disaster recovery procedures",p:"Deploy end-to-end pipeline with monitoring, practice failover and recovery scenarios"},
{d:100,t:"Interview prep part 1: advanced SQL patterns (top-N, running totals, dedup, hierarchy, gaps/islands)",p:"Solve 15 advanced SQL problems, practice explaining query optimization"},
{d:101,t:"Interview prep part 2: database design, HA/replication, BigQuery cost/performance trade-offs",p:"Mock interviews: design scenarios, explain architecture choices, discuss trade-offs"},
{d:102,t:"Interview prep part 3: performance tuning, monitoring, debugging slow queries",p:"Practice query profiling, index design, and performance optimization deep dives"},
{d:103,t:"Interview prep part 4: distributed systems concepts, sharding, consistency, eventual consistency in BigQuery",p:"Discuss distributed database concepts, BigQuery's eventual consistency model"},
{d:104,t:"Full mock assessment: comprehensive test spanning all 6 phases (40 questions mixed difficulty)",p:"Take full mock assessment including MySQL operational topics, BigQuery analytics, and design scenarios"},
{d:105,t:"FINAL REVIEW: personal project review + career readiness assessment",p:"Review your capstone project, create portfolio summary, plan continued learning path",r:true},
];

function fallbackCopy(text){
  const ta = document.createElement('textarea');
  ta.value = text;
  ta.style.position = 'fixed';
  ta.style.opacity = '0';
  document.body.appendChild(ta);
  ta.select();
  try{ document.execCommand('copy'); }catch(e){}
  document.body.removeChild(ta);
}

let completed = new Set();
let activePhase = 0; // 0 = all

function loadState(){
  const saved = localStorage.getItem('sqlBqTrackerState');
  if(saved){
    try{
      const arr = JSON.parse(saved);
      completed = new Set(arr);
    } catch(e){ console.error('Failed to load state:', e); }
  }
}

function saveState(){
  localStorage.setItem('sqlBqTrackerState', JSON.stringify(Array.from(completed)));
}

function phaseOf(day){
  return phases.find(p => day>=p.range[0] && day<=p.range[1]).id;
}

function renderTabs(){
  const tabsEl = document.getElementById('tabs');
  tabsEl.innerHTML = '';
  const allTab = document.createElement('div');
  allTab.className = 'tab' + (activePhase===0 ? ' active':'');
  allTab.textContent = 'All Days';
  allTab.onclick = () => { activePhase=0; renderTabs(); renderDays(); };
  tabsEl.appendChild(allTab);
  phases.forEach(p=>{
    const t = document.createElement('div');
    t.className = 'tab' + (activePhase===p.id ? ' active':'');
    t.textContent = p.name;
    t.onclick = () => { activePhase=p.id; renderTabs(); renderDays(); };
    tabsEl.appendChild(t);
  });
}

let openDay = null;

function renderDays(){
  const list = document.getElementById('dayList');
  list.innerHTML = '';
  const filtered = activePhase===0 ? days : days.filter(d=>phaseOf(d.d)===activePhase);
  filtered.forEach(day=>{
    const card = document.createElement('div');
    card.className = 'day-card' + (completed.has(day.d)?' done':'') + (day.r?' review':'') + (openDay===day.d?' open':'');
    card.innerHTML = `
      <div class="day-head">
        <div class="checkbox" data-check="${day.d}">
          <svg viewBox="0 0 24 24" fill="none" stroke="#0a0a0f" stroke-width="3"><path d="M20 6L9 17l-5-5"/></svg>
        </div>
        <div class="day-num">D${day.d}</div>
        <div class="day-topic">${day.t}${day.r?'<span class="review-tag">Review</span>':''}</div>
        <div class="chevron">&#9656;</div>
      </div>
      <div class="day-detail">
        <div class="detail-block">
          <span class="k">Learning · 1hr <button class="copy-btn" data-copy="learn-${day.d}">Copy</button></span>
          <span class="detail-text">${day.t}</span>
        </div>
        <div class="detail-block practice">
          <span class="k">Practice · 1hr <button class="copy-btn" data-copy="practice-${day.d}">Copy</button></span>
          <span class="detail-text">${day.p}</span>
        </div>
      </div>
    `;
    card.querySelector('[data-check]').onclick = (e)=>{
      e.stopPropagation();
      if(completed.has(day.d)) completed.delete(day.d); else completed.add(day.d);
      saveState();
      renderDays(); renderStats();
    };
    card.querySelectorAll('.copy-btn').forEach(btn=>{
      btn.onclick = (e)=>{
        e.stopPropagation();
        const [kind] = btn.dataset.copy.split('-');
        const text = kind==='learn' ? `Day ${day.d} - Learning: ${day.t}` : `Day ${day.d} - Practice: ${day.p}`;
        const finish = ()=>{
          btn.textContent = 'Copied';
          btn.classList.add('copied');
          setTimeout(()=>{ btn.textContent='Copy'; btn.classList.remove('copied'); }, 1500);
        };
        if(navigator.clipboard && navigator.clipboard.writeText){
          navigator.clipboard.writeText(text).then(finish).catch(()=>{
            fallbackCopy(text); finish();
          });
        } else {
          fallbackCopy(text); finish();
        }
      };
    });
    card.onclick = ()=>{
      openDay = openDay===day.d ? null : day.d;
      renderDays();
    };
    list.appendChild(card);
  });
}

function renderStats(){
  const done = completed.size;
  const pct = Math.round((done/105)*100);
  document.getElementById('statDone').textContent = done;
  document.getElementById('statPct').textContent = pct+'%';
  document.getElementById('progressFill').style.width = pct+'%';
  let curPhase = 1;
  for(const p of phases){
    const phaseDays = days.filter(d=>phaseOf(d.d)===p.id);
    const phaseDone = phaseDays.filter(d=>completed.has(d.d)).length;
    if(phaseDone < phaseDays.length){ curPhase = p.id; break; }
    curPhase = p.id;
  }
  document.getElementById('statPhase').textContent = curPhase+'/6';
}

loadState();
renderTabs();
renderDays();
renderStats();
</script>
</body>
</html>
