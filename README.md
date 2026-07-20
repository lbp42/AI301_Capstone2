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

The `transformCube` function in `transform-meta-extended.ts` builds the 
object returned by the `/meta` API endpoint. It includes `sql` but never 
adds `sql_table`, so even though the data exists in `cubeDefinitions`, 
it never makes it into the response.

### Proposed Solution

Add `sql_table` to the return object of `transformCube` so it gets 
included in the API response alongside `sql`.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** The `/meta` endpoint is missing `sql_table` because 
`transformCube` doesn't include it in its return object.

**Match:** The `sql` field is already handled the same way — 
`sql: cubeDefinitions[cube?.name]?.sql` — so `sql_table` follows 
the exact same pattern.

**Plan:** [Step-by-step implementation plan]
1. Add `sql_table: cubeDefinitions[cube?.name]?.sql_table` to the 
   `transformCube` return object in `transform-meta-extended.ts`
2. Update `transform-meta-extended.test.ts` to add a test case 
   verifying `sql_table` appears in the output
33. Add a test case to `transform-meta-extended.test.ts` that checks 
   `sql_table` is included in the `transformCube` output when it 
   exists in `cubeDefinitions`
4. Add a test case verifying `sql_table` is `undefined` in the output 
   when it is not defined in `cubeDefinitions`

**Implement:** [[Link](https://github.com/lbp42/cube/tree/fix-issue-8711)

**Review:** Will follow the project's contribution guidelines and 
match the existing code style.
**Evaluate:** Run the existing test suite to confirm nothing breaks, 
and verify the new test case passes.

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: `transformCube - includes sql_table when defined` — verifies 
  that when `sql_table` exists in `cubeDefinitions`, it appears in the 
  `transformCube` output with the correct value
- [ ] Test case 2: `transformCube - sql_table is undefined when not defined` — 
  verifies that when `sql_table` is not in `cubeDefinitions`, the output 
  returns `undefined` for that field

### Integration Tests

- [ ]Not applicable for this change — the fix is isolated to a single helper 
  function and fully covered by unit tests

### Manual Testing

- Ran the full test suite with `npm test -- --testPathPattern=transform-meta-extended`
- All 198 tests passed across 11 test suites
- Confirmed `transform-meta-extended.ts` maintains 97.43% code coverage

---

## Implementation Notes

### Week [1] Progress

Located the file `transform-meta-extended.ts` after discovering the original 
filename from the issue had been renamed. Found the `transformCube` function 
and identified that `sql_table` was simply missing from its return object.

### Week [2&3] Progress

Added `sql_table: cubeDefinitions[cube?.name]?.sql_table` to the `transformCube` 
function. Added two unit tests following the existing test patterns. Built the 
project using lerna and ran the full test suite to confirm all tests pass.

### Code Changes

- **Files modified:** - `packages/cubejs-api-gateway/src/helpers/transform-meta-extended.ts`
  - `packages/cubejs-api-gateway/test/helpers/transform-meta-extended.test.ts`
- **Key commits:** https://github.com/lbp42/cube/tree/fix-issue-8711
- **Approach decisions:** Followed the exact same pattern as the existing 
  `sql` field since `sql_table` requires no transformation — it's passed 
  through directly unlike `sql` which uses `stringifyMemberSql`

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
