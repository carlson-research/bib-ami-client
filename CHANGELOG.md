# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [Unreleased] - 2026-09-09

### Changed
* **Clean Architecture Refactor:** Separated the core library into distinct Domain, Data, and Presentation layers, enforcing inward dependency flow via protocol interfaces.
* **Provider Fallback Cascade:** Refactored `CitationRepository` to orchestrate automatic metadata lookup cascades across CrossRef, DataCite, arXiv, and OpenLibrary.
* **Test Suite Modernization:** Upgraded test infrastructure using `httpx_mock`, pushing overall coverage to 87% with 95%+ coverage across repository modules.

### Removed
* **Enterprise LLM Functions:** Removed non-core LLM methods (`resolve_fuzzy_citation`, `validate_bibtex_string`, `harmonize_graph_citations`) from `BibTexManager` to isolate them for the upcoming `bib-ami-enterprise` extension.
* **Scaffolding Cleanup:** Removed legacy query planning methods (`build_query_plan`, `_determine_strategy`) from `CitationRepository`.

## [0.21.0] - 2026-09-07

### Added
* **Automated Release Workflow:** Relocated `Makefile` to the repository root and added the `make release version=X.Y.Z` target to automate version staging, git committing, annotated tagging, and pushing.
* **Release Documentation:** Added `RELEASING.md` establishing the maintainer release checklist and pre-flight verification runbook.
* **Automated GitHub Releases:** Integrated `softprops/action-gh-release` into the PyPI deployment workflow to automatically create formal GitHub Releases with attached build distributions (`dist/*`).

### Changed
* **Dynamic Version Resolution:** Configured `pyproject.toml` and `doc/conf.py` to derive package versions dynamically from `src/bib_ami/_version.py`.
* **CI/CD Quality Gates:** Upgraded `.github/workflows/tests.yml` to execute `pre-commit` linting and `mypy` static type checking prior to test execution.
* **Sphinx Documentation Settings:** Updated `doc/conf.py` to use native `autodoc_typehints = "description"` for cleaner type hint rendering.

### Fixed
* **Git Tooling Compatibility:** Pinned `pre-commit<3.8.0` in `pyproject.toml` dev dependencies to maintain compatibility with system Git installations prior to v2.36.0.
* **Type Checker Pragmatism:** Adjusted `mypy` configuration (`warn_return_any = false`) to streamline static analysis across dynamic external API clients.
* **Test Fixture Imports:** Resolved `ruff` module-level import (`E402`) warnings across test fixtures.

## [0.20.0] - 2026-09-07

### Added
* **ArXiv Preprint Client:** Added `ArXivClient` and the `PreprintClient` Protocol interface to validate arXiv identifiers via the Atom XML API and retrieve manuscript metadata.
* **Preprint to Journal Automatic Upgrades:** The `Validator` now detects if an arXiv preprint has been subsequently published in a peer-reviewed journal and automatically upgrades the citation record with the formal journal DOI.
* **Extended Citation Record Schema:** Added support for `eprint`, `archiveprefix`, `arxivid`, and `verified_eprint` fields in the core Pydantic `CitationRecord` model.
* **Configurable User-Agent Strings:** `CrossRefClient`, `DataCiteClient`, `OpenLibraryClient`, and `ArXivClient` now accept a `user_agent` parameter during initialization to override default request headers.

### Changed
* **Centralized Version Control:** Extracted package version tracking into a standalone `_version.py` module to eliminate circular dependencies. API clients now dynamically bind their default User-Agent headers to the active package version.

## [0.19.0] - 2026-09-07

### Added
* **Author Name Canonicalizer:** Normalizes author strings into a standard `Last, First` BibTeX format during initial `CitationRecord` Pydantic validation, preserving lowercase particles ('von', 'van', 'de') and brace-protected institutional names.
* **Legacy ISBN-10 to ISBN-13 Converter:** Automatically recalculates modulo-10 check digits and converts 10-digit ISBNs into standardized 13-digit ISBNs in `Validator` prior to OpenLibrary queries, recording conversions in `audit_info`.
* **LaTeX Math & Special Character Sanitizer:** Added `clean_latex_text` utility to translate BibTeX accent macros (`{\'{e}}`, `{\"{o}}`) to standard UTF-8 characters while isolating and protecting inline math mode blocks (`$...$`) across title, booktitle, and journal fields.
* **Source Code Bundler Utility:** Added `scripts/bundle_source.py` to bundle project source files into a unified file for offline context ingestion.

## [0.18.0] - 2026-09-06

### Added
* **Resolvable Capability Protocol:** Added `is_resolvable` method to the `DoiClient` protocol, standardizing network reachability checks across `CrossRefClient` and `DataCiteClient`.

### Changed
* **Explicit Dependency Injection:** Refactored `BibTexManager.__init__` constructor signature to require explicit client parameters (`crossref_client`, `datacite_client`, `openlibrary_client`) instead of a generic `client` keyword argument.
* **Network Logic Abstraction:** Decoupled `Validator` from `httpx` primitives and raw HTTP status handling, delegating DOI reachability checks directly to `DoiClient` implementations.
* **Type-Safe Test Doubles:** Standardized `MockCrossRefClient` and `MockDataCiteClient` to fulfill structural protocols statically, eliminating type warnings and runtime patching in the test suite.

### Removed
* **Dynamic Client Inspection:** Removed obsolete `hasattr` and `getattr` dynamic checks for network methods inside `Validator`.

## [0.17.0] - 2026-09-05

### Added
* **Multi-Source Verification & Enrichment:** Integrated `DataCiteClient` for datasets and software, and `OpenLibraryClient` for ISBN book lookups.
* **Centralized CLI Configuration:** Added the `bib-ami config` sub-command with `set`, `get`, and `list` actions to manage user defaults persistently.
* **Offline Network Mocking for Tests:** Implemented offline HTTP mocking in the pytest suite, ensuring fast, deterministic testing without live network calls.

### Changed
* **CI/CD Pipeline Migration:** Consolidated testing and deployment workflows from CircleCI to a unified GitHub Actions pipeline.
* **Triage Logic Refactoring:** Standardized `resolve_fuzzy_citation` in `BibTexManager` to delegate directly to the `Triage` class rather than reproducing its logic.
* **Codebase Cleanup:** Cleaned up all pipeline modules (`step_1`, `step_2`, `step_3`), removing outdated comments, legacy project artifacts (`scorelp`), and unnecessary history notes.

### Removed
* **GPL Dependencies:** Completely removed `fuzzywuzzy` and `python-Levenshtein` to maintain permissive licensing (replaced with MIT-licensed `rapidfuzz`).
* **Legacy HTTP Client:** Removed `requests` in favor of the modern, async-ready `httpx` library.
