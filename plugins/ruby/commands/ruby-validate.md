Run full validation suite for a Ruby 3.2 project: dependency audit, linting, and tests.

## Steps

### 1. Ruby version check
Verify the project targets Ruby 3.2:
```bash
ruby --version
```
If `.ruby-version` or `Gemfile` specifies a different major/minor version, warn the user before continuing.

### 2. Dependency audit
```bash
bundle audit check --update
```
- If `bundler-audit` is not installed: `gem install bundler-audit` then retry.
- Report any vulnerable gems with their CVE IDs and recommended fix versions.
- Fail fast if any **critical** severity CVEs are found.

### 3. RuboCop
```bash
bundle exec rubocop --format progress
```
- If a `.rubocop.yml` is present, it will be picked up automatically.
- Report offense count by cop category (Layout, Lint, Style, etc.).
- On failure, list the top offenses with file:line references so the user can jump directly to them.

### 4. RSpec
```bash
bundle exec rspec --format documentation --color
```
- Report: total examples, failures, pending.
- On failure, print the full failure messages and backtraces.
- If `spec/` directory doesn't exist, report it and skip this step.

## Summary

After all steps complete, print a single status table:

| Check           | Status  | Details                        |
|-----------------|---------|--------------------------------|
| Ruby version    | ✓ / ✗   | found version                  |
| Bundle audit    | ✓ / ✗   | N vulnerabilities              |
| RuboCop         | ✓ / ✗   | N offenses                     |
| RSpec           | ✓ / ✗   | N examples, N failures         |

Exit with a clear PASSED or FAILED verdict. If any step fails, list the specific actions the developer should take to fix it.
