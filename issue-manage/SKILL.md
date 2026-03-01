---
name: issue-manage
description: Record, categorize, and gain insights from issues you encounter during your work. Use this skill when the user wants to log an issue, view an issue dashboard, or generate insight reports about past issues. Trigger with `/issue-manage:record`, `/issue-manage:dashboard`, or `/issue-manage:insight`.
---

# Issue Manage

A skill for recording, categorizing, and learning from issues across tech, business, and process domains. This skill helps identify recurring problems and automatically distills lessons learned.

## Overview

This skill has three main slash commands:
1. `/issue-manage:record` - Log a new issue.
2. `/issue-manage:dashboard` - View a matrix of recorded issues.
3. `/issue-manage:insight` - Generate a detailed insight report.

Issues are stored in an `issues.json` file in the directory where the command is run. If the file does not exist, it will be created automatically.

## Storage Format

The `issues.json` file should follow this structure:
```json
[
  {
    "id": "ISSUE-1",
    "timestamp": "2023-10-27T10:00:00Z",
    "description": "Spring Boot application fails to start due to missing bean definition.",
    "domain": "tech",
    "sub_category": "spring"
  }
]
```

## Commands

### 1. `/issue-manage:record`

When the user runs this command, they want to log a new issue.

**Instructions:**
1. Ask the user for the issue description if they haven't provided one in their prompt.
2. Automatically determine the `domain` and `sub_category` based on the description.
3. Generate a unique `id` (e.g., `ISSUE-<N>`) and the current `timestamp`.
4. Append the new issue to `issues.json` in the current directory.

**Categorization Guidelines:**
- **domain**: Must be one of `tech`, `business`, or `process`.
- **sub_category**: Be specific. 
  - *Tech examples*: `spring`, `react`, `database`, `design-pattern`, `aws`, `library-bug`, etc.
  - *Business examples*: `requirements-gap`, `logic-error`, `edge-case`, etc.
  - *Process examples*: `deployment-failure`, `communication-gap`, `ci-cd`, etc.

**Example User Prompt:**
> /issue-manage:record My Spring Boot app keeps crashing on startup because it can't find the DataSource bean.

**Action:** You read/create `issues.json`, append a new entry with domain `tech` and sub-category `spring`, and confirm to the user.

---

### 2. `/issue-manage:dashboard`

When the user runs this command, they want a high-level overview of all recorded issues.

**Instructions:**
1. Read the `issues.json` file in the current directory.
2. If the file is empty or doesn't exist, politely inform the user that no issues have been logged yet.
3. Display a summary of the issues using a Markdown matrix table, grouped by domain and sub-category. Include the counts.

**Expected Output Format:**
```markdown
## Issue Dashboard

| Domain | Sub-Category | Issue Count | Recent Issues |
| :--- | :--- | :---: | :--- |
| Tech | spring | 2 | ISSUE-1: Bean missing, ISSUE-3: Circular dep |
| Tech | database | 1 | ISSUE-2: Connection timeout |
| Process | ci-cd | 1 | ISSUE-4: Pipeline stalled |
```

---

### 3. `/issue-manage:insight`

When the user runs this command, they want a deep dive into the recorded issues, categorized by domain, to extract lessons and best practices.

**Instructions:**
1. Read the `issues.json` file.
2. Analyze the issues and group them by `domain` and `sub_category`.
3. Generate a Markdown file named `insight-YYYYMMDD.md` (using the current date) in the same directory as `issues.json`.
4. The generated file MUST contain:
    - An executive summary.
    - A matrix table summarizing the issues (similar to the dashboard).
    - Detailed insights for each domain/sub-category. For each group, explicitly state:
        - **What we learned** from these issues.
        - **Preventive measures** to avoid them in the future.
        - **Best practice advice** relevant to the category.
5. After generating the file, notify the user that it has been created and optionally display a brief summary in the chat.

**Expected File Structure (`insight-YYYYMMDD.md`):**

```markdown
# Issue Insights - YYYY-MM-DD

## Executive Summary
A brief summary of the overall health and major trends seen in recent issues.

## Issue Matrix
| Domain | Sub-Category | Issue Count | Key Theme |
| :--- | :--- | :---: | :--- |
| Tech | spring | 2 | Configuration and Dependency Injection |
| ... | ... | ... | ... |

## Detailed Insights

### Domain: Tech
#### Category: spring

**Issues:**
- ISSUE-1: Spring Boot application fails to start due to missing bean definition.
- ISSUE-3: Circular dependency detected between Service A and Service B.

**What we learned:**
- Relying too heavily on field injection can mask architectural flaws until runtime.
- Component scanning can sometimes miss beans if packages are not structured correctly.

**Preventive measures:**
- Shift to constructor injection for all Spring components to ensure dependencies are resolved at startup and circular dependencies are caught early.
- Centralize complex bean configurations in `@Configuration` classes rather than scattering `@Component` annotations.

**Best practice advice:**
- Always write a minimal `@SpringBootTest` context load test to catch startup failures during the CI phase rather than at deployment time.
```
