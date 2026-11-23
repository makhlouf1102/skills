---
name: coding
description: Diagnose, plan, implement, review, and verify codebase changes for bugs, design issues, and performance problems. Use a coordinated multi-agent workflow with independent design-focused reviewers and a clear implementation handoff.
---

# Coding

Use this workflow for codebase problems. Keep conclusions tied to repository evidence and the user's requirements. The lead agent owns the plan, integration, review loop, verification, and final explanation.

## Required companion skill

This workflow uses `$software-design-philosophy` for design reviews. Before starting a review, confirm that the skill is installed for the current user. If it is missing, install it globally with:

```sh
npx skills add wondelai/skills/software-design-philosophy --global
```

## Model selection

At the start of each coding task, before planning or launching agents, ask the user which model should run the orchestrator and which model or models to use for implementation and verifier subagents. Offer only models available in the current environment, and wait for the user's choice before proceeding. If the user delegates the choice, select supported defaults and state them. If the requested orchestrator model cannot be applied to the current thread, explain the limitation and ask whether to continue with the current model or restart under the requested one.

## Diagnose and plan

- Read applicable `AGENTS.md` instructions, inspect the working tree, and trace the relevant implementation, callers, data flow, and existing checks before editing. Preserve unrelated user changes.
- State the problem in observable terms: expected behavior, actual behavior, and a minimal reproduction or example when possible. Separate confirmed causes from hypotheses.
- Identify affected files, modules, interfaces, downstream behavior, and dependencies between work items.
- Before making changes, write a concrete plan. Include the proposed approach, ordered work items, files or boundaries each item owns, agent assignments, integration points, and how the result will be verified. Update the plan if investigation changes the scope.
- Split implementation work across multiple agents when the work can be divided into independent, safely integrable pieces. Give each agent a bounded assignment and avoid concurrent edits to the same files. If the change is too small or coupled to split safely, assign one implementation owner and use another agent for focused investigation or integration support; state why the code work stayed with one owner.
- Define the verification plan before implementation, including expected outcomes and important edge cases. Follow user and repository instructions about which checks may be run. If important details are missing, state reasonable assumptions or ask the user when they materially affect the approach.

## Compare solutions

- For nontrivial changes with multiple viable approaches, list the real options and their tradeoffs. Do not pad the list with speculative variants.
- Evaluate options against task-specific criteria, including mandatory requirements, correctness, fit with the verification plan, maintainability, compatibility, implementation effort, and regression risk as relevant. Treat mandatory requirements as gates; use clear weights and evidence for scored criteria. Mark estimates as estimates.
- Choose the strongest viable approach and explain why the alternatives rank lower. For a straightforward change with only one sensible approach, say so briefly instead of manufacturing a scoring exercise.

## Implement and integrate

- Implement the selected approach in the planned work areas. Keep changes cohesive, follow local conventions, and avoid unrelated cleanup, dependencies, or abstractions without a concrete need.
- Coordinate delegated work through clear ownership and integration boundaries. Review each contribution in the full repository context, resolve conflicts, and ensure the integrated result still matches the plan.
- If findings or implementation evidence change the design, update the plan and explain the decision before proceeding.

## Independent design review and fix loop

- After implementation, have two independent verifier agents review the current changes. Both must use `$software-design-philosophy` and assess the affected design and code against the problem, requirements, confirmed cause, architecture, plan, and likely regressions.
- Ask each verifier for specific, evidence-backed findings and concrete suggestions. Each review must state the current score against the skill's eight Quick Diagnostic rows, identify failed rows, and name the changes that would raise the affected design toward 10/10. Distinguish issues introduced by this change from pre-existing or out-of-scope observations.
- Keep the verifiers independent: neither should rely on the other's conclusions. Do not let implementation agents approve their own fixes.
- Route actionable findings to implementation agent(s), or fix them in the lead agent's owned work. After fixes, have the same two verifiers independently review the updated change again. Repeat this review → fix → re-review cycle until both verifiers explicitly confirm there are no remaining in-scope findings or actionable suggestions and approve the result.
- Re-review after every material code change. Resolve reviewer disagreements with repository evidence and another focused review; do not treat disagreement as approval.
- If a verifier is unavailable, delegation is unavailable, or a finding cannot be resolved within scope, do not claim the two-verifier gate passed. Explain the limitation or blocker and the remaining work. Do not silently replace an unavailable verifier with the implementer.

## Verify

- Run the agreed, in-scope checks after implementation and after any material fix, following the user's instructions and repository constraints. Investigate failures, fix causes, and rerun affected checks. Report exact commands and outcomes; distinguish passed, failed, and unavailable checks.
- A clean design review does not substitute for functional verification, and passing checks do not substitute for design review.

## Handoff

Explain the decisions and outcome clearly. Include:

- The problem and confirmed cause, with remaining uncertainty separated from facts.
- The selected approach, alternatives considered when relevant, and why the decision was made.
- The resulting architecture: affected modules, their responsibilities, interfaces, and important data or control flow.
- The code changes and which work areas or agents handled them.
- Verification commands and outcomes, plus the final status and findings of both verifiers.
- Potential blockers, unresolved risks, deferred suggestions, or unavailable validation. If none remain, say so.
