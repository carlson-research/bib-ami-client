# bib-ami

A lightweight, thin-client CLI for the **bib-ami** citation verification service. This package routes all citation metadata lookups and verification logic to the proprietary backend engine.

## Installation

Install the client directly from PyPI:

```bash
pip install bib-ami
```

## Configuration

To authenticate with the backend API, set your API key as an environment variable:

```bash
export BIB_AMI_API_KEY="your-secret-api-key"
```

## Usage

The client provides two primary commands via the terminal:

### 1. Citation Lookup
Query citation metadata using a DOI, arXiv ID, or standard reference identifier:

```bash
bib-ami lookup 10.1038/nature12345
```

### 2. Launch Web App
Open the interactive `bib-ami` web application in your default system browser:

```bash
bib-ami launch
```
*(You can optionally pass a custom URL using `bib-ami launch --url https://custom.bib-ami.com`)*

### Help
View all available commands and options:

```bash
bib-ami --help
```

## Development & Testing

This project uses `pyproject.toml` for dependency management and requires Python 3.11+.

Clone the repository and install the development dependencies in editable mode:

```bash
git clone [https://github.com/your-org/bib-ami-client.git](https://github.com/your-org/bib-ami-client.git)
cd bib-ami-client
pip install -e ".[dev]"
```

**Run the Test Suite:**
Execute the complete unit and end-to-end test suite with terminal coverage reporting:

```bash
pytest tests/ --cov=bib_ami --cov-report=term-missing
```

**Code Quality:**
This project enforces strict formatting and linting via `pre-commit`. Install the git hooks to run automatically before every commit:

```bash
pre-commit install
```
