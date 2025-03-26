---
date: '2025-04-26T21:28:19+07:00'
draft: true
title: 'GitHub Badges Guide'
summary: 'A comprehensive guide to adding GitHub badges to your repository'
categories:
  - Code
tags:
  - GitHub
  - Markdown
  - Documentation
---

## Common Badge Types

### 1. Workflow Status Badge

Monitor your GitHub Actions workflow status:

```markdown
![Workflow Status](https://github.com/{username}/{repo}/actions/workflows/{workflow-name}.yml/badge.svg?branch=main)
```

### 2. Release Badge

Display your latest release version:

```markdown
![GitHub Release](https://img.shields.io/github/v/release/{username}/{repo}?include_prereleases)
```

### 3. Repository Information Badges

Show various repository statistics:

```markdown
# Latest Release
![GitHub Release](https://img.shields.io/github/release/{username}/{repo}.svg)

# License Information
![GitHub License](https://img.shields.io/github/license/{username}/{repo}.svg)

# Repository Size
![GitHub Repo Size](https://img.shields.io/github/repo-size/{username}/{repo}.svg)

# Primary Language
![GitHub Top Language](https://img.shields.io/github/languages/top/{username}/{repo}.svg)
```

## Usage Tips

1. Replace `{username}` with your GitHub username
2. Replace `{repo}` with your repository name
3. For workflow badges, replace `{workflow-name}` with your actual workflow file name

## Additional Resources

- [Shields.io](https://shields.io/) - Create custom badges
- [GitHub Actions Badges](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/adding-a-workflow-status-badge)
