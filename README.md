# Nemi Yossef Hai — Backend Portfolio

Three backend projects in Python that I designed and built end to end: APIs, databases, message queues for real-time event processing, and search powered by language models. Each project starts with a single Docker command and comes with tests and CI.

---

## Projects

### wiki-guard — Real-Time Wikipedia Edit Monitor
Watches the live stream of Wikipedia edits across all languages, detects suspicious patterns such as vandalism and edit wars, and shows the alerts and the stream's activity on a live dashboard.

At its core is a Kafka message queue: one process reads the edit stream and publishes each edit to the queue, and a separate process consumes the messages and checks them against the detection rules. Taking in the stream and analyzing it are independent of each other, so load on one side does not bring down the other.

- **Stack:** FastAPI, Kafka, PostgreSQL, Redis, Server-Sent Events, Docker
- **Repository:** [wikiguard-py](https://github.com/nemiys/wikiguard-py)

### tinyurl-py — URL Shortener with Smart Links
A URL shortener with smart links: one short link can send phones to a different address than desktops, and can stop working at a set time. Every click is counted, and the owner sees clicks per day.

Click counting goes through a Redis Streams message queue: the visitor gets the redirect immediately, and each click enters the queue as an event. A separate worker reads the events through a consumer group, writes them to Cassandra, and acknowledges each event only after it is written. Counting never slows down the redirect, and no click is lost or counted twice, even when the worker goes down.

- **Stack:** FastAPI, MongoDB, Redis, Redis Streams, Cassandra, JWT, argon2, Docker
- **Repository:** [tinyurl-py](https://github.com/nemiys/tinyurl-py)

### labor-rights-rag-py — Labor Rights Q&A (RAG)
Answers a citizen's questions about employment rights in Israel, in Hebrew, from the text of 20 labor laws. Every answer cites the sections it used and comes with a verification card built in code from the database, never by the model: the law, the section, the literal quote, the date of the text, and a link to the law in the Knesset's legislation database. The search is hand-written and measured against a fixed set of evaluation questions, including a comparison with answering without search and with putting all the laws in the prompt.

> Not legal advice — the source text is a volunteer-edited wiki, not the official gazette, and the system says so in every answer.

- **Stack:** FastAPI, PostgreSQL + pgvector, BGE-M3 embeddings, LLM answers through a provider-agnostic interface (Gemini in the evaluation), Docker
- **Repository:** [labor-rights-rag-py](https://github.com/nemiys/labor-rights-rag-py)

---

## Skills

Everything listed here is used in at least one of the projects.

| Category | Tools | Where |
|---|---|---|
| Backend | FastAPI (async), Pydantic, SQLAlchemy 2 (async), Alembic | all three |
| Databases | PostgreSQL, pgvector, MongoDB, Redis, Cassandra | wiki-guard, tinyurl, labor-rights |
| Message queues & streaming | Kafka, Redis Streams (consumer groups), Server-Sent Events | wiki-guard, tinyurl |
| Search & RAG | hand-written vector, full-text and BM25 search, RRF fusion, BGE-M3 embeddings, LLM API integration, retrieval evaluation (hit@k, MRR) | labor-rights |
| Auth & Security | JWT, argon2 password hashing, rate limiting, SSRF protection | tinyurl; rate limiting also in labor-rights |
| Testing & Quality | pytest, mypy (strict), ruff, GitHub Actions CI | all three |
| Infrastructure | Docker, Docker Compose, uv | all three |

---

## About Me

Python backend developer who builds data systems, and knows how to analyze them. I work with message queues and streaming (Kafka, Redis Streams), databases (PostgreSQL, MongoDB, Cassandra, Redis), and search powered by language models. On the analysis side, I write advanced SQL, work with pandas and build dashboards in Tableau, from the business question to the recommendation.

Alongside the three projects above, my [analytics portfolio](https://github.com/nemiys/data-analytics-portfolio) includes a conversion-funnel analysis in Python, a marketing performance analysis in SQL and an HR dashboard in Tableau.

How I work: every design decision is documented with its reason. Before I decide, I compare alternatives and measure the trade-offs. In the labor-rights system, for example, I compared the retrieval I built against two alternatives and weighed accuracy against cost and response time. I develop with Claude Code, and I own the review and the testing: every change passes automated tests before it is merged.

📫 [LinkedIn](https://www.linkedin.com/in/nemi-yossef-hai) · nemiys@gmail.com
