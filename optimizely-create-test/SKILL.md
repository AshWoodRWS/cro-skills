---
name: optimizely-create-test
description: Create and configure Optimizely Web Experimentation draft tests for RWS and Trados from a brief or a generated test idea. Use for requests such as create RWS-001, build a Trados CTA test, use the next TRA number, or name and create an experiment in Opal. Apply brand-specific naming, live ID checks, targeting, metric selection, variation implementation and QA through available Opal agents and tools.
---

# Optimizely Test Creator

Execute this workflow in Optimizely Opal. Create a draft experiment when requested, rather than stopping at a plan when the necessary tools and details are available.

A skill provides instructions. It does not itself grant API access, install tools or guarantee access to other agents.

## Use existing context

Use the installed `ab-testing` skill for hypothesis quality, test design and interpretation when available.

Use relevant CRO, copywriting, analytics and variation-development skills or agents when available. Do not invent their identifiers or assume they can be invoked.

Apply this skill's RWS/Trados naming and creation rules to operational setup.

Match the statistical method configured in Optimizely. Do not impose fixed-horizon stopping rules on a sequential method.

Read the current request, accessible brief, account/project context and approved team defaults before asking questions.

Ask only for missing information required for the next action. Bundle related questions.

Do not require baseline traffic or conversion rate simply to create a draft. Require them for a numerical duration or sample-size estimate.

## Identify intent and brand

- RWS tests use `RWS`.
- Trados tests use `TRA`.
- Use an explicit brand first. Otherwise infer it from a verified rws.com or trados.com page and project context.
- Ask if brand or project is ambiguous.
- Preserve an explicit test ID after normalising its format: `RWS001` becomes `RWS-001`.
- If the request contains only an ID, look for a matching brief or backlog item in accessible sources.
- If no matching brief exists, ask for the page and intended change, or whether to generate an idea.
- Do not invent a brief from an ID.
- Distinguish ideation-only requests from requests to create a test.
- Do not create experiments for ideation-only requests.

If asked to generate and build an idea:

1. Inspect the page and available research.
2. Develop one focused hypothesis.
3. Build the requested draft when implementation details are sufficiently grounded.
4. Label assumptions and never fabricate evidence.

## Allocate the test number

Maintain independent RWS and TRA sequences.

Never hardcode a next number from a screenshot.

1. Identify the approved master numbering source from team context.
2. If unspecified, check accessible Optimizely experiment records and any available tracker. Report the source used.
3. Ask which source is authoritative when records conflict.
4. Read all relevant records, following pagination and covering the brand's relevant projects.
5. Include drafts, active, paused, completed and accessible archived tests, plus reserved IDs in the approved tracker.
6. Extract numeric IDs from matching brand prefixes.
7. For an automatic ID, choose one greater than the highest allocated number. Do not fill historical gaps.
8. Pad to at least three digits, allowing numbers beyond 999.
9. Recheck immediately before creation. If concurrent allocation changes the next available number, recalculate.
10. Use an atomic reservation mechanism if the numbering source provides one. A read-only check cannot guarantee collision-free allocation.

For a supplied ID already in use:

- Show the matching record.
- Ask whether to use a new ID or work on the existing draft.
- Do not overwrite or duplicate automatically.

If authoritative records cannot be accessed, leave the automatic ID unassigned and explain the missing access.

Do not claim a number is available without checking.

Do not silently renumber a user-supplied ID.

Never modify a running experiment as part of this creation workflow.

## Write the experiment name

Use:

`[PREFIX]-[NUMBER] - [Page/product] - [Test change] - [Optional audience or version]`

Rules:

- Use spaces around separator hyphens.
- Keep names specific and concise.
- Preserve supplied product and page labels.
- Use team terms such as `LP`, `CTA` and `Social proof` when appropriate.
- Add `V2`, `V3`, etc. only for a verified iteration or explicit user instruction.
- Give iterations a new test ID.
- Add an audience qualifier when it distinguishes targeting.

Observed style examples, not available IDs:

- `RWS-189 - Contact page - Social proof - G2/Forrester vs Client logos V3`
- `RWS-186 - Language Weaver LP - Scalable AI - New block`
- `RWS-178 - Localization Translation LP - NA Audience only`

Illustrative Trados name, not an existing experiment:

- `TRA-[next verified number] - Trados Solutions LP - Main CTA copy`

In a tracker with separate Test-ID and Test name columns:

- Put the ID in Test-ID.
- Omit its repetition from Test name.
- Use the full combined name in Optimizely.

## Define the test

Write a short description in plain UK English. Identify the page, change and expected measurable outcome.

Use a hypothesis such as:

`Because [verified observation, or explicitly labelled assumption], we believe [change] will increase [primary outcome] for [audience].`

Do not invent numerical uplift predictions.

Do not imply that a bolder design guarantees faster statistical significance.

Prefer one focused change. For a combined redesign, state that the test measures the package rather than each component's contribution.

Document:

- Brand and verified project.
- Exact target URL and page conditions.
- Unchanged control.
- Variation changes.
- One primary metric using a verified existing event/metric ID.
- Relevant secondary and guardrail metrics where available.
- Audience, device and locale targeting.
- Experiment traffic allocation.
- Variation weights as a separate setting.

Use approved team/project defaults where they exist.

If no approved variation split exists, propose 50/50 between control and one variation. Identify it as a proposed default.

Do not guess total experiment exposure, event IDs, geographic definitions or project IDs.

For an enquiry-generation test, prefer a verified enquiry/form-submission metric if available. Ask if there are several plausible events.

Use CTA clicks as a secondary metric where appropriate rather than automatically replacing the downstream business outcome.

## Build in Optimizely

1. Inspect available Opal tools and agent capabilities.
2. Use existing experiment-creation/configuration and variation-development tools when supported.
3. Use a custom API tool only when already configured and authorised.
4. Never request credentials in chat or put secrets in GitHub files.
5. Verify the intended project, page and metric records before creating anything.
6. Inspect the current page and actual DOM before producing selector-dependent JavaScript/CSS.
7. Preserve form validation, accessibility, responsive layout and existing tracking.
8. Do not generate selectors from a screenshot alone.
9. Build the unchanged control and requested variation.
10. Reuse approved implementation patterns where available.
11. Limit changes to the target page and intended elements.
12. Create/configure the experiment as a draft using the verified ID, full name, description, variations, metrics, targeting and approved allocation.
13. Record the returned experiment ID immediately.
14. If creation succeeds but configuration fails, continue with the created draft where possible.
15. Do not create a second experiment on retry.
16. Report partial completion accurately.
17. Read back the saved experiment.
18. Compare its name, project, status, targeting, metrics, allocation and variation implementation with the brief.
19. Update the approved tracker with the ID, name, actual status and experiment link if the workflow authorises tracker writes.
20. If tracker access is unavailable, provide the row to add and state that it was not saved.

When required tools are unavailable:

- Produce the complete draft specification.
- Provide variation code where the page has been sufficiently inspected.
- Identify the specific missing capability.
- Distinguish a prepared specification from an experiment actually created in Optimizely.

## QA and launch

Preview control and variations on supported desktop and mobile views when preview tools are available.

Check:

- Intended changes.
- Responsive layout.
- Console errors.
- Targeting.
- Control integrity.
- Metric firing using available evidence.

Report unperformed checks rather than marking them passed.

Treat synthetic submissions and other external effects separately from visual checks.

Keep the experiment in draft unless the user explicitly requests launch.

A creation request alone is not a launch request.

When launch is explicitly requested, complete available QA and required account checks before proceeding through supported tools.

Report blockers accurately.

Do not claim success from generated code alone.

## Return a concise result

Return:

- Full test ID and name.
- One-sentence hypothesis.
- Control and variation summary.
- Project and target page.
- Primary metric.
- Experiment exposure and variation split.
- Actual state: created draft, partially configured draft, or specification only.
- Optimizely experiment link, if returned or verifiably constructed from supported account context.
- Completed QA.
- Unresolved settings or checks.

Do not invent an experiment link.

Do not claim a tracker update that did not occur.

Do not describe a draft as live.
