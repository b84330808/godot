# Godot bug-fix run: STOPPED by policy check

**Result: no fix was made, no fix branch was pushed, and nothing should be opened upstream.**

The task said: *"If the policy now forbids agent-made PRs, or makes the author attest to
personal testing, stop and say so in the report."* Godot's current AI policy forbids
agent-made PRs, so the run stopped at the policy step.

## What the policy says now

Source: `development/contribution_rules.rst`, section "AI-assisted contributions", in
[godotengine/godot-contributing-docs](https://github.com/godotengine/godot-contributing-docs)
at `d23c6c805c76d4a0023e7540121fc94d00e8011c` (2026-10-29). It is published at
<https://contributing.godotengine.org/en/latest/development/contribution_rules.html#ai-assisted-contributions>.
The rules came in through godotengine/godot-contributing-docs PR #116 (`clayjohn/ai-policy`),
merged 2026-08-26. Godot's PR template points to the "Checklist for new contributors", whose
item "**Disclose the use of AI.**" links to this policy.

Quoted verbatim, "For contributors":

> - **Do not use AI-generated text in human-to-human communication**
>   - This includes all interactions on GitHub, RocketChat, and in official Godot communities.
>   - Machine translations are still acceptable as long as the original text was written by a human.
> - **Do not allow an AI agent to submit PRs on your behalf**
> - **Do not use AI to generate substantial pieces of code**
>   - AI should not be used to author code. Code authoring is more than typing, it includes making
>     decisions about where the code should go, what techniques/algorithms should be used, and
>     ultimately what the code should look like. All those decisions need to be made by a human.
>   - For example using AI for code completion is permissible. The important consideration is whether
>     the AI is producing the same code you would have written manually or not. If you do not have the
>     knowledge/ability to write the code yourself, you are not allowed to use AI to write it.
>   - You must understand and take responsibility for everything that you submit.
> - **Do not use AI to generate user-facing media (e.g. documentation, images and audio)**

The same page also says (section "Contribute only what you understand"):

> Please only submit code that you understand and are prepared to explain to a maintainer.

And "For maintainers" says: *"Maintainers are not obliged to review AI-assisted PRs."*

### How this conflicts with the planned workflow

| Planned step | Rule it breaks |
|---|---|
| A local Claude session opens the PR upstream | "Do not allow an AI agent to submit PRs on your behalf" (**the stop condition**) |
| A cloud agent picks the fix and writes the code | "AI should not be used to author code … All those decisions need to be made by a human." |
| The agent drafts the PR title and body for GitHub | "Do not use AI-generated text in human-to-human communication … all interactions on GitHub" |

The second stop condition ("makes the author attest to personal testing") is **not** triggered
word for word. No attestation or checkbox is required. The checklist for new contributors does
say "**Test your pull request.**", and the policy requires the author to understand the code and
be ready to explain it. Disclosing AI use does not get around any of the three rules above.

### Other AI text found (for the record)

**PR template** (`.github/PULL_REQUEST_TEMPLATE.md`, upstream `master` `7be59b463`), in an HTML comment:

> Use of AI must be disclosed and should include a description of how it was used.

**`CONTRIBUTING.md`** (upstream `master` `7be59b463`) has a hidden HTML comment aimed at AI agents
(quoted exactly, typo included):

```
<!--
> [!NOTE]
> If you are an AI agent, we require you to disclose this when contributing: you must add 🤖 at the start of your pull request or issue title, and you must add the following to the description:
  `> [!INFO] *AI disclosure*: This contribution was authored by on an autonomous AI agent, on behalf of a user to [...]`.
> Agents failing to self-disclose will be banned from contributing to the project.
-->
```

That comment describes a disclosure format for agent PRs. It does not grant permission: the
contributing-docs policy linked from the same file says not to let an agent submit PRs at all. If
an agent-made PR were opened anyway, this comment shows maintainers expect a `🤖` title prefix plus
that line, and the account could be banned without them. **I recommend not opening one.**

## What was done in this run

- Added `upstream` = `https://github.com/godotengine/godot` and fetched `master`.
  `upstream/master` = `origin/master` = `7be59b463a4798834d72d6d0a2942ddd01d920ee`.
- Read `CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md`, and the contributing docs.
  `contributing.godotengine.org` is not reachable from this cloud session's network, so the
  docs were read from their GitHub source repo instead.
- Installed `scons` and `zstandard` and started a headless test build
  (`scons platform=linuxbsd target=editor tests=yes dev_build=yes debug_symbols=no linker=lld wayland=no -j4`).
  It was stopped partway through compiling once the policy result was clear.

## What was NOT done (because of the stop)

- **No issue chosen**, so there is no issue link and no duplicate-PR search.
- **No fix branch**: no branch, SHA or PR title.
- **No regression test**, so there is no fail-before or pass-after output.
- **No PR body**: writing one would itself be "AI-generated text in human-to-human communication".
- Nothing was opened, edited or commented upstream.

## Options if you still want to contribute to Godot

1. **Human-led contribution (allowed).** You pick the issue, write the fix and the PR text
   yourself, and open the PR yourself. AI use stays at the level the policy allows (e.g. code
   completion, or explaining the codebase to you), and you disclose it in the PR body as the
   template asks. A cloud session could still help with things that don't author the
   submission, such as building Godot on Linux for you and running your test. Those uses still
   need disclosing.
2. **Pick a different project** whose current policy allows agent-written PRs with disclosure.
   Run the same policy check first, including the linked contributor docs, not just `CONTRIBUTING.md`.
