# Release Checklist & Workflow

Follow these steps when preparing a new version release for `bib-ami`.

### 1. Pre-Release Verification
Run local checks to ensure clean builds, full test coverage, and strict linting:

```bash
pytest
pre-commit run --all-files
```

### 2. Version & Changelog Update
1. Update `__version__` in `src/bib_ami/_version.py`.
2. Move items from `## [Unreleased]` into a new version header (e.g., `## [0.20.0] - 2026-09-07`) in `CHANGELOG.md`.

### 3. Commit, Tag, and Publish
Use the automated `Makefile` target to stage, commit, tag, and push the release to GitHub. Pushing the tag will automatically trigger the CI/CD pipelines to publish to PyPI and update Read the Docs.

```bash
make release version=X.Y.Z
```

*For reference, this automation executes the following manual commands:*

```bash
# Stage and commit version bump
git add src/bib_ami/_version.py CHANGELOG.md
git commit -m "bump: release vX.Y.Z"

# Create annotated tag
git tag -a vX.Y.Z -m "Release vX.Y.Z"

# Push branch and tag
git push origin master
git push origin vX.Y.Z
```
