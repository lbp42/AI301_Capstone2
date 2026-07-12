# AI301_Capstone2

# Contribution [2]: Meta API (/meta): Add the sql_table field to the response
 #8711]

**Contribution Number:** [2]  
**Student:** [Amanda Orozco]  
**Issue:** [GitHub Issue #8711](https://github.com/cube-js/cube/issues/8711)
**Status:** [Phase II]

---

## Why I Chose This Issue

I picked this issue because it's a small, focused change that lets me get 
experience in a TypeScript codebase. The `/meta` API is just missing one 
field that should already be there, so the scope is really clear. It also 
felt like a good follow up to my first contribution since I wanted to try 
something in a different language.

---

## Understanding the Issue

### Problem Description

The `/v1/meta` API endpoint in Cube.js is missing the `sql_table` field 
in its response. Even when using the `?extended` query parameter, the field 
doesn't show up, even though the similar `sql` field does.

### Expected Behavior

The `/meta` endpoint should return the `sql_table` field alongside the 
existing `sql` field so users can see which SQL table a cube maps to.

### Current Behavior

When you call `/v1/meta` or `/v1/meta?extended`, the response includes 
`sql` but not `sql_table`, making it very difficult to get that info from 
the API.

### Affected Components

`packages/cubejs-api-gateway/src/helpers/transformMetaExtended.ts` — 
specifically the `transformCube` function around line 37.

---

## Reproduction Process

### Environment Setup

- Cloned fork on macOS (Apple Silicon)
- Set up SSH authentication for GitHub
- No build required to reproduce — issue is visible by reading the source code

### Steps to Reproduce

1. Clone the repository
2. Open `packages/cubejs-api-gateway/src/helpers/transform-meta-extended.ts`
3. Look at the `transformCube` function — it includes `sql` but not `sql_table`
4. The `/v1/meta?extended` endpoint calls this function, so `sql_table` never 
   makes it into the API response

### Reproduction Evidence

- **Branch link:** https://github.com/lbp42/cube/tree/fix-issue-8711
- **My findings:** The `transformCube` function in 
  `transform-meta-extended.ts` returns `sql` from `cubeDefinitions` but 
  is missing `sql_table`. Adding one line — 
  `sql_table: cubeDefinitions[cube?.name]?.sql_table` — to the return 
  object should fix it. The test file 
  `transform-meta-extended.test.ts` will also need to be updated.

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
