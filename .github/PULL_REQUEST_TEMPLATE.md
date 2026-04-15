<!--
BEFORE SUBMITTING: Read every word of this template. PRs that leave
sections blank, contain multiple unrelated changes, or show no evidence
of human involvement will be closed without review.
-->

## What problem are you solving?
<!-- Describe the specific problem you encountered as a Godot developer
     using godot-superpowers. What were you doing, what went wrong or
     was missing? If this was a session issue, include what the AI did
     wrong and ideally a transcript.

     "Improving" something is not a problem statement. What broke? What
     failed? What was the Godot workflow experience that motivated this? -->

## What does this PR change?
<!-- 1-3 sentences. What, not why — the why belongs above. -->

## Is this change appropriate for godot-superpowers core?
<!-- This fork contains:
     - Godot-specific skills that benefit any Godot developer regardless of project
     - Updates to upstream skills to work better in Godot workflows (e.g. TDD → GUT)
     - Fork infrastructure (README, CLAUDE.md, plugin.json)

     It does NOT contain:
     - Skills for specific Godot addons, frameworks, or third-party tools (those belong as separate plugins)
     - Project-specific or workflow-specific configuration
     - General (non-Godot) skill changes (submit those upstream to obra/superpowers)

     Does your change fit the first list? If not, where does it belong? -->

## What alternatives did you consider?
<!-- What other approaches did you try or evaluate before landing on this one?
     Why were they worse? If you didn't consider alternatives, say so. -->

## Does this PR contain multiple unrelated changes?
<!-- If yes: stop. Split it into separate PRs.
     If you believe the changes are related, explain the dependency. -->

## Existing PRs
- [ ] I have reviewed all open AND closed PRs for duplicates or prior art
- Related PRs: <!-- #number, #number, or "none found" -->

## Environment tested

| Harness | Harness version | Model | Model version/ID |
|---------|-----------------|-------|------------------|
|         |                 |       |                  |

## Evaluation
- What was the initial prompt that surfaced this problem?
- How many sessions did you run AFTER making the change?
- How did outcomes change compared to before the change?

<!-- "It works" is not evaluation. Describe the before/after difference
     you observed across multiple sessions. -->

## Rigor

- [ ] If this modifies a skill: I used `superpowers:writing-skills` and completed adversarial pressure testing (paste results below)
- [ ] All GDScript examples are typed (type annotations on variables, params, and return types)
- [ ] Signal examples use Godot 4 syntax
- [ ] Test runner examples use GUT or gdUnit4 (not npm/TypeScript)
- [ ] I did not modify Red Flags tables, iron laws, or rationalization lists without eval evidence

## Human review
- [ ] A human has reviewed the COMPLETE proposed diff before submission

<!--
STOP. If the checkbox above is not checked, do not submit this PR.
-->
