# bib-ami: Development Roadmap

This document outlines the planned features, architectural enhancements, and quality-of-life improvements for future releases of the open-source `bib-ami` library.

---

## Phase 1: Academic & Math Citation Integrity

Features focused on refining citation accuracy for complex academic bibliographies, particularly in mathematical and scientific domains.

### 1. Retraction & Expression of Concern Detection
* **Rationale:** Citation integrity requires flagging papers that have been formally retracted or issued expressions of concern.
* **Functionality:** Utilize CrossRef's `is-referenced-by-count` and update metadata arrays to detect retracted papers, automatically tagging them as `Suspect (Retracted Paper)` during validation.

---

## Phase 2: Ingestion & Document Integration

Enhancements to optimize how bibliographies are discovered and scoped prior to validation.

### 1. TeX Document Citation Harvester
* **Rationale:** When compiling a specific LaTeX paper, processing a master `.bib` database of 5,000 entries is inefficient.
* **Functionality:** Extend `Ingestor` to scan `.tex` files, extract active citation keys (`\cite{}`, `\citep{}`, `\autocite{}`), and restrict pipeline ingestion to only the referenced records rather than processing massive master databases.

### 2. Multi-Format Support (CSL-JSON & BibLaTeX)
* **Rationale:** Modern reference managers (Zotero, Mendeley) and publication platforms increasingly standardize on CSL-JSON and extended BibLaTeX schema.
* **Functionality:** Add support for ingesting CSL-JSON files in the `Ingestor` and exporting to CSL-JSON in the `Writer` to support modern reference managers and publication platforms.

---

## Phase 3: Data Quality & Architecture

Refining record reconciliation, standardizing metadata schema, and decoupling network transport.

### 1. Interactive "Gleaning" Mode
* **Rationale:** Manual review of unverified citations is currently a disconnected process.
* **Functionality:** Introduce an interactive CLI prompt for the `suspect.bib` output, allowing users to manually `[k]eep`, `[d]iscard`, or `[s]earch again` for suspect entries.

### 2. Configurable Triage Rules
* **Rationale:** Different disciplines have different standards for acceptable unverified sources.
* **Functionality:** Move the rules for what constitutes an "Accepted" entry (e.g., `@book`, `@techreport`) into the configuration file to allow custom triage logic.

### 3. Dedicated Network Transport Abstraction (`HttpClient` Protocol)
* **Rationale:** Domain clients currently wrap `httpx.Client` directly. Enterprise plugins or offline proxies needing custom connection pooling or caching should not have to subclass whole clients.
* **Functionality:** Extract an explicit `HttpClient` protocol to separate raw HTTP transport operations (`head`, `get`) from domain capabilities (`is_resolvable`), cleanly isolating low-level I/O from semantic metadata queries.
