---
name: code-review
description: Comprehensive code review checklist for pull requests
---

When reviewing code, the git history in comm/ should be used unless otherwise
specified.

# Code Review Checklist

When reviewing code, check each of these areas:

## Functionality
- [ ] Code does what the PR description claims
- [ ] Edge cases are handled
- [ ] Error handling is appropriate

## Code Quality
- [ ] Follows project style guide
- [ ] No hardcoded values that should be configurable
- [ ] Functions are focused and well-named

## Testing
- [ ] New functionality has tests
- [ ] Tests are meaningful, not just for coverage

