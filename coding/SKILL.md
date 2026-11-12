---
name: coding
description: Diagnose and solve codebase problems, including bugs, design issues, and performance problems. Map affected code and causes, define tests, compare solutions, implement the best-ranked option, and verify it.
---

# Coding

Use this workflow for codebase problems. Keep conclusions tied to evidence from the repository and the user's requirements.

## Diagnose the problem

- Read applicable `AGENTS.md` instructions, inspect the working tree, and trace the relevant implementation, callers, data flow, and existing tests before editing. Preserve unrelated user changes.
- State the problem in observable terms: expected behavior, actual behavior, and a minimal reproduction or example when possible.
- Identify the affected files, modules, interfaces, and downstream behavior. Separate confirmed causes from plausible hypotheses; gather evidence before treating a hypothesis as the root cause.
- Define how the fix will be tested before choosing a solution. Include reproduction steps, expected results, relevant existing tests or commands, and important edge cases. If details needed to make this plan meaningful are unavailable, state assumptions or ask the user.

## Compare solutions

- List the distinct, viable solutions and their tradeoffs. Do not pad the list with speculative variants.
- Build an evaluation grid whose criteria and weights fit this task. Include the user's stated preferences and requirements, expected correctness, fit with the test plan, and other relevant factors such as performance, maintainability, implementation effort, compatibility, or regression risk.
- Treat mandatory requirements as gates. For the remaining criteria, assign clear weights and scores (for example, 1–5), cite concrete evidence for each score, and calculate a weighted total. State when a score or weight is an estimate. If priorities are unclear and would change the winner, ask the user before selecting.
- Treat test results as evidence only after running the tests; before implementation, describe expected test fit without claiming a solution passes. Explain the selected top-ranked solution and briefly document why the alternatives scored lower.

## Implement, review, and verify

- Implement the top-ranked solution only. Keep the change cohesive, follow local conventions, and avoid unrelated cleanup, dependencies, or abstractions without a concrete need.
- After implementation, ask two independent critic agents to review the change against the problem statement, requirements, root-cause analysis, evaluation criteria, test plan, and likely regressions. Ask for specific findings and evidence, not general approval.
- Correct review findings and have both agents review the revised implementation again. Continue until both explicitly approve and report no remaining findings. If they disagree, investigate the evidence and resolve the disagreement before considering review complete. Never claim two-agent review if two independent agents were unavailable; report that limitation.
- Run the defined test plan, including relevant focused checks and regression cases. If a check fails, investigate and fix the cause, rerun affected checks, and repeat the critic review for any material code change. Report commands and outcomes accurately; distinguish unavailable checks from passing checks.

## Handoff

Summarize the problem, confirmed cause, affected code, test plan and results, solution alternatives, evaluation grid and weights, selected solution, implementation, and the two critics' final status. Call out unresolved uncertainty or unavailable validation directly.
