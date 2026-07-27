# AI301_Capstone2

# Contribution [2]: Meta API (/meta): Add the sql_table field to the response
 #8711]

**Contribution Number:** [2]  
**Student:** [Amanda Orozco]  
**Issue:** [GitHub Issue #8711](https://github.com/cube-js/cube/issues/8711)
**Status:** [Phase IV]

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

**PR Link:** [[GitHub PR URL when submitted]](https://github.com/cube-js/cube/pull/11360)

**PR Description:** The /meta and /meta?extended endpoints were missing the sql_table field in their response, even though the similar sql field was already included. This made it difficult to determine which SQL table a cube maps to via the API. This PR adds sql_table to the transformCube function's return object in transform-meta-extended.ts, following the exact same pattern already used for sql. Added two unit tests verifying the field appears when defined and is undefined when not defined. Ran the full test suite scoped to the modified file — all 198 tests passed across 11 test suites, maintaining 97.43% code coverage.

**Maintainer Feedback:**
- [07/26/2026]: No feedback received yet. The PR was just opened; a review was automatically requested from the code-owning team via the project's CODEOWNERS setup, and CI workflows are currently pending maintainer approval (standard practice for external/community contributors on this repo).

**Status:** Awaiting review

---

## Learnings & Reflections

### Technical Skills Gained

Working through this issue helped me get more comfortable with Git commands beyond the basics — things like reconciling divergent branches, choosing between merge and rebase strategies, and understanding what actually happens during a fast-forward vs. a three-way merge. I also came away with a better sense of how files in a large codebase "talk" to each other — tracing how a small helper function like transformCube feeds into a public-facing API response, rather than looking at files in isolation.


### Challenges Overcome
The issue pointed to a filename that no longer existed in the codebase — it had been renamed at some point to transform-meta-extended.ts. I brainstormed different ways to track down where the logic had actually moved to, since a direct search for the old filename wouldn't have worked. Writing the two unit tests, on the other hand, was straightforward once I found the file — I followed the existing test patterns already in place for the sql field and adapted them for sql_table.

### What I'd Do Differently Next Time

I'd sync my fork's master branch (and any feature branches) with upstream right before starting a new issue, rather than after finishing the code — that way I'd avoid the "157 commits behind" surprise altogether when I go to open the PR. I'd also capture my actual terminal test output at the time I run it, rather than reconstructing it from memory afterward, since real output is stronger evidence in a PR than a paraphrased summary.
Also try to confirm file paths/names against the current codebase earlier, instead of assuming the issue description was still accurate."
---

## Resources Used

GitHub Docs: About pull requests — https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests
