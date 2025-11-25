# Performance Improvement Guidelines

This document outlines common performance anti-patterns and recommended improvements for code in this repository.

## GitHub Actions Workflow Optimizations

The CI workflow (`.github/workflows/blank.yml`) has been optimized with the following improvements:

### 1. Concurrency Control
```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```
**Why:** Cancels redundant in-progress workflow runs when new commits are pushed, saving CI minutes and reducing queue times.

### 2. Shallow Clone
```yaml
- uses: actions/checkout@v4
  with:
    fetch-depth: 1
```
**Why:** Fetches only the latest commit instead of full git history, significantly speeding up checkout for large repositories.

### 3. Job Timeout
```yaml
timeout-minutes: 10
```
**Why:** Prevents runaway jobs from consuming resources indefinitely.

### Additional Workflow Recommendations

- **Cache dependencies:** Use `actions/cache` for package managers (npm, pip, etc.)
- **Parallel jobs:** Split independent tasks into parallel jobs
- **Conditional execution:** Use `paths` filters to skip workflows when irrelevant files change

## General Code Performance Guidelines

### Algorithm Efficiency
- Use appropriate data structures (Set/Map for lookups, arrays for iteration)
- Avoid nested loops when possible - consider using hash maps for O(1) lookups
- Cache expensive computations that are reused

### Memory Management
- Avoid creating unnecessary object copies
- Use streams for large data processing
- Release references to large objects when no longer needed

### I/O Operations
- Batch database queries instead of N+1 queries
- Use connection pooling for database connections
- Implement caching for frequently accessed data
- Use async/parallel I/O when possible

### Frontend Performance
- Lazy load components and routes
- Minimize bundle size with tree shaking
- Use virtualization for long lists
- Debounce/throttle expensive event handlers

## Configuration Improvements Made

### VSCode Launch Configuration
The `.vscode/launch.json` has been updated to:
- Use `http://localhost:3000` as a standard development server URL instead of hardcoded paths
- Remove user-specific file paths that don't work across different machines
- Improve portability for team collaboration
