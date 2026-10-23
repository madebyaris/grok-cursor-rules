# Changes to grok-cursor-rules

Goal: make the Cursor agent finish more work per turn without asking, and prove the work with real checks. All original content is kept unless listed here.

## 1. New always-on rule: `grok-act.mdc`
Why: no rule tells the agent how to work the loop. `grok-think` says "Verify with types, tests, or the real behavior" but never says how to find the commands, when to run them, or what to do when they fail. Nothing mentions to-dos or next steps.
What: find test/lint/typecheck/build commands first (package.json, Makefile, CI, AGENTS.md); to-do list for 3+ steps; run the narrowest check after each edit; fix own failures without asking; separate pre-existing failures; full checks before reporting; "Done means" checklist; ban on "want me to proceed?".

## 2. `grok-hitl.mdc`: explicit "do without asking" list and a default to keep going
Why: "Decide alone" only lists "a local variable name, an error shape...". The agent stays unsure about reading, running tests, or editing files, so it asks.
What: listed always-reversible actions (read, search, run tests/lint/build, edit working tree, fix own failures). Added a stop case for missing secrets, paid services, or accounts. Added "Stopping is the exception. If none of these cases applies, do not end the turn with a question." Added: end with up to three next steps and do the first one if it is small and reversible.

## 3. `grok-visual.mdc` and `grok-invent.mdc`: `alwaysApply: true` to `false` (Apply Intelligently)
Why: both are design-only ("Skip it for logic-only changes", "Skip this on coding-track work") yet loaded on every coding turn. That spends context and pulls a bug fix toward design talk. Cursor docs: `alwaysApply: false` + `description` (no globs) = the agent pulls the rule in when the description fits.
What: flipped to false and rewrote the descriptions to say when to read them ("Read this before calling any UI ... done", "Read this on open briefs ..."). `grok-think` design step 13 now tells the agent to read both, so they still load on design work.

## 4. `grok-eval.mdc`: new step 7 "Report"
Why: step 6 says "It compiles is not evidence" but nothing forces the agent to show the evidence.
What: name the commands run and results; name any check not run and why; never write "done" over a failing or unrun check. Taste steps renumbered 8 to 12. Taste section now says to read `grok-visual` first (it is no longer always on).

## 5. `grok-think.mdc`: links to the new loop
What: points to `grok-act` at the top and in coding step 6; design step 13 loads `grok-visual` and `grok-invent`.

## 6. README
- Badges and table updated (5 always on, 4 on demand), `grok-act` added.
- Quick start "use in every project" step replaced. It copied `.mdc` files into `~/.cursor/rules/`, but Cursor's rules docs (cursor.com/docs/context/rules, checked 23 Oct 2026) describe User Rules as plain text in Customize > Rules and do not document a global `~/.cursor/rules` folder. Not proven broken; flagged as unverified.
- Added a test prompt for `grok-act`.

## Not changed, and why
- `grok-write`, `grok-image`, `grok-plan`: frontmatter is already correct (plan and image use description-only, which matches Cursor's "Apply Intelligently").
- No `globs` added: the rules are about task type, not file type, so descriptions fit better.
- No `AGENTS.md` added: repo-specific commands belong in each target project's own `AGENTS.md`, which `grok-act` now reads.

## Open suggestions (not applied)
- Always-on context is still about 2,300 words (act, think, eval, hitl, write). If Grok still drifts, move the design track of `grok-think` into its own description-only rule.
- Run the README test prompts and record results; the README badge still says "untested".
