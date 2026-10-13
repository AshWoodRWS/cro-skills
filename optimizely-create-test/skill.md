---
name: optimizely-create-test
description: Create and configure Optimizely Web Experimentation draft tests for RWS and Trados from a brief or a generated test idea. Use for requests such as create RWS-001, build a Trados CTA test, use the next TRA number, or name and create an experiment in Opal. Apply brand-specific naming, live ID checks, targeting, metric selection, variation implementation and QA through available Opal agents and tools.
---

# Optimizely Test Creator

Execute this workflow in Optimizely Opal. Create a draft experiment when requested, rather than stopping at a plan when the necessary tools and details are available. Treat a skill as instructions: it does not itself grant API access, install tools or guarantee access to other agents.

## Use existing context

Use the installed `ab-testing` skill for hypothesis quality, test design and interpretation when available. Use relevant CRO, copywriting, analytics and variation-development skills or agents if they are available. Do not invent their identifiers or assume they can be invoked. Apply this skill's RWS/Trados naming and creation rules to operational setup. Match the statistical method configured in Optimizely; do not impose fixed-horizon stopping rules on a sequential method.

Read the current request, accessible brief, account/project context and approved team defaults before asking questions. Ask only for missing information required for the next action, bundling related questions. Do not require baseline traffic or conversion rate simply to create a draft; require them for a numerical duration or sample-size estimate.

## Identify intent and brand

- RWS tests use `RWS`; Trados tests use `TRA`.
- Use an explicit brand first; otherwise infer from a verified rws.com or trados.com page and project context. Ask if brand or project is ambiguous.
- Preserve an explicit test ID after normalising its format: `RWS001` becomes `RWS-001`.
- If the request contains only an ID, look for a matching brief or backlog item in accessible sources. If none exists, ask for the page and intended change, or whether to generate an idea. Do not invent a brief from an ID.
- Distinguish ideation-only requests from requests to create a test. Do not create experiments for ideation-only requests.
- If asked to generate and build an idea, inspect the page and available research, propose one focused hypothesis and build the requested draft when implementation details are sufficiently grounded. Label assumptions; never fabricate evidence.

## Allocate the test number

Maintain independent RWS and TRA sequences. Never hardcode a next number from a screenshot.

1. Identify the approved master numbering source from team context. If it is unspecified, use accessible Optimizely experiment records and any available tracker to check IDs; report the source used. Ask which source is authoritative when they conflict.
2. Read all relevant records, following pagination and covering the brand's relevant projects. Include drafts, active, paused, completed and accessible archived tests, plus reserved IDs in the approved tracker.
3. Extract numeric IDs from matching brand prefixes. For an automatic ID, choose one greater than the highest allocated number; do not fill historical gaps. Pad to at least three digits, allowing numbers beyond 999.
4. Recheck immediately before creation. If concurrent allocation changes the next available number, recalculate. Use an atomic reservation mechanism if the numbering source provides one; a read-only check cannot guarantee collision-free allocation.
5. For a supplied ID already in use, show the matching record and ask whether to use a new ID or work on the existing draft. Do not overwrite or duplicate automatically.
6. If authoritative records cannot be accessed, leave the automatic ID unassigned and explain the missing access. Do not claim a number is available without checking.

Do not silently renumber a user-supplied ID. Never modify a running experiment as part of this creation workflow.

## Write the experiment name

Use:

`[PREFIX]-[NUMBER] - [Page/product] - [Test change] - [Optional audience or version]`

Use spaces around separator hyphens. Keep names specific and concise. Preserve supplied product and page labels. Use team terms such as `LP`, `CTA` and `Social proof` when appropriate. Add `V2`, `V3`, etc. only for a verified iteration or explicit user instruction; iterations receive a new test ID. Add an audience qualifier when it distinguishes targeting.

Observed style examples, not available IDs:

- `RWS-189 - Contact page - Social proof - G2/Forrester vs Client logos V3`
- `RWS-186 - Language Weaver LP - Scalable AI - New block`
- `RWS-178 - Localization Translation LP - NA Audience only`

Illustrative Trados name, not an existing experiment:

- `TRA-[next verified number] - Trados Solutions LP - Main CTA copy`

In a tracker with separate Test-ID and Test name columns, put the ID in Test-ID and omit its repetition from Test name. Use the full combined name in Optimizely.

## Define the test

Write a short description in plain UK English: identify the page, change and expected measurable outcome. Use a hypothesis such as:

`Because [verified observation, or explicitly labelled assumption], we believe [change] will increase [primary outcome] for [audience].`

Do not invent numerical uplift predictions or imply a bolder design guarantees faster significance. Prefer one focused change. For a combined redesign, state that the test measures the package rather than the contribution of each component.

Document:

- Brand, verified project and exact target URL/page conditions.
- Unchanged control and variation changes.
- One primary metric using a verified existing event/metric ID.
- Relevant secondary/guardrail metrics where available.
- Audience, device and locale targeting.
- Experiment traffic allocation and variation weights as distinct settings.

Use approved team/project defaults where they exist. If no approved split exists, propose 50/50 between control and one variation and identify it as a proposed default. Do not guess total experiment exposure, event IDs, geographic definitions or project IDs. For an enquiry-generation test, prefer a verified enquiry/form-submission metric if available; ask if there are several plausible events. CTA clicks may be a secondary metric rather than replacing the downstream business outcome.

## Choose form-submission scope

Support these two configurations using the verified existing `Form Submission` event:

| Metric name | Event-property filtering | Measure |
| --- | --- | --- |
| Form Submission - on page | Mandatory: `hubspot_form_id` is the exact discovered form ID | Increase in unique conversions per visitor |
| Form - sitewide | None | Increase in unique conversions per visitor |

Use the on-page metric as primary for a test whose main outcome is submission of a specific form. Add the sitewide metric when requested; do not add it to every test automatically. Use sitewide as primary when the brief explicitly identifies broader form conversions as the main outcome. If scope or primary designation is unclear, ask whether to use the specific form, sitewide submissions or both.

The sitewide metric measures tracked Form Submission events attributed to experiment visitors, not every visitor on the website. Verify project and tracking scope. Do not describe unique conversions per visitor as a total count of all submissions.

Only the deliberately selected sitewide metric may omit the form filter. Never save an on-page form metric unfiltered, even as a fallback. If the intended form ID cannot be verified, leave its metric setup incomplete and report the gap.

## Discover the form and configure its metric

For an on-page form-fill primary metric:

1. Inspect the exact target page's rendered DOM using an available browser/DOM tool. Wait for dynamically loaded forms; a text-only summary is insufficient.
2. Find `data-form-id` values on forms and HubSpot containers. Deduplicate repeated values on wrappers and nested forms. Do not confuse the form ID with a portal ID, DOM ID or instance ID.
3. Match the form to the brief using its heading, position and purpose. If several different forms could match, ask which one to measure.
4. Resolve the existing `Form Submission` event in the correct Optimizely project. Verify its actual event ID/API name; do not use its display name as an API identifier.
5. Verify from accessible tracking code or runtime evidence that successful submissions send the event property `hubspot_form_id` with the discovered value. The DOM attribute alone does not prove tracking exists.
6. Configure the primary metric as:
   - Type: Custom Event Metric.
   - Name: `Form Submission - on page`.
   - Event: verified existing `Form Submission` event.
   - Direction: Increase.
   - Measure: Unique conversions per visitor.
   - Event-property filter: `hubspot_form_id` is the discovered form ID.
7. Save through an available metric-configuration tool and read back the settings. Reuse the existing submission event rather than creating a duplicate event for each form ID.
8. If control and variation use different form IDs, include both through a supported OR filter or another verified measurement design. Do not count only the control's form.
9. If the same form ID is reused on other pages, the ID filter does not uniquely identify this page. Use verified page-property filtering where page-specific attribution is required and supported.
10. If tracking is absent, identify the gap and prepare tracking using a verified successful-submission callback. Do not equate submit-button clicks or an unverified native submit event with successful form fills. Avoid duplicate listeners.
11. Do not submit a real enquiry to test tracking without authorisation for that external effect. Report tracking checks that remain unverified.
12. If DOM inspection or metric configuration is unavailable, report the specific missing capability and provide the exact settings where possible. Do not claim a metric was saved.

Example only: `data-form-id="1d2ec5ad-eb6a-4925-a61f-44f6ccfd66b6"` maps to the filter `hubspot_form_id is 1d2ec5ad-eb6a-4925-a61f-44f6ccfd66b6`. Never reuse this example ID without inspecting the target page.

## Build in Optimizely

1. Inspect available Opal tools and agent capabilities. Use existing experiment-creation/configuration and variation-development tools when supported. Use a custom API tool only when it is already configured and authorised. Never request credentials in chat or put secrets in GitHub files.
2. Verify the intended project, page and metric records before creating anything.
3. Inspect the current page and actual DOM before producing selector-dependent JavaScript/CSS. Preserve form validation, accessibility, responsive layout and existing tracking. Do not generate selectors from a screenshot alone.
4. Build the unchanged control and requested variation. Reuse approved implementation patterns where available. Keep changes limited to the target page and intended elements.
5. Create/configure the experiment as a draft, using the verified ID, full name, description, variations, metrics, targeting and approved allocation. Record the returned experiment ID immediately.
6. If creation succeeds but configuration fails, continue with the created draft where possible. Do not create a second experiment on retry. Report partial completion accurately.
7. Read back the saved experiment and compare its name, project, status, targeting, metrics, allocation and variation implementation with the brief.
8. Update the approved tracker with the ID, name, actual status and experiment link if the workflow authorises tracker writes. If tracker access is unavailable, provide the row to add and state that it was not saved.

When required tools are unavailable, produce the complete draft specification and available variation code, then identify the specific missing capability. Distinguish a prepared specification from an experiment actually created in Optimizely.

## QA and launch

Preview control and variations on supported desktop and mobile views when preview tools are available. Check intended changes, responsive layout, console errors, targeting, control integrity and metric firing using available evidence. Report unperformed checks rather than marking them passed. Treat synthetic submissions and other external effects separately from visual checks.

Keep the experiment in draft unless the user explicitly requests launch. A creation request alone is not a launch request. When launch is explicitly requested, complete available QA and required account checks before proceeding through supported tools; report blockers accurately. Do not claim success from generated code alone.

## Return a concise result

Return:

- Full test ID and name.
- One-sentence hypothesis and control/variation summary.
- Project, target page, primary metric and traffic split.
- Actual state: created draft, partially configured draft, or specification only.
- Optimizely experiment link returned or verifiably constructed from supported account context, if available.
- Metric scope (on-page/sitewide), primary and secondary designation, and exact saved form-ID filters.
- Completed QA and any unresolved settings or checks.

Do not invent an experiment link, claim a tracker update that did not occur, or describe a draft as live.
