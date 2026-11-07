# Setup Version Policy

An agent skill for establishing a project's canonical version source and deciding when completed changes require a version increment.

The skill guides an agent through reviewing existing version information, confirming a starting version with the user, and documenting a policy suited to the project's release stage.

## Usage

Use [SKILL.md](SKILL.md) as the instructions for an agent working in the project whose version policy you want to establish or review. If the skill is available in your agent's environment, request it by name:

```text
Use setup-version-policy to establish this project's canonical VERSION
source and document when completed changes require a version increment.
```

For an existing policy:

```text
Use setup-version-policy to review this project's version sources and
bump rules. Identify conflicting version authorities and unclear rules.
```

This repository contains instructions and agent metadata. There is no application to build or executable setup script to run.

## Workflow

1. **Inspect the project.** Read applicable project guidance and look for version candidates in the root `VERSION`, package manifests, runtime constants, release notes, and Git tags. Distinguish the current product version from dependency and component versions.
2. **Confirm the starting version.** Ask the user to adopt a discovered candidate, request a proposal based on shipped milestones in commit history, or supply a version. Confirmation is required even when only one plausible candidate exists.
3. **Establish the canonical source.** Once the user chooses, use the repository-root `VERSION` as the single source of truth. If an existing policy names another authority, resolve that conflict with the user before changing it.
4. **Document increment rules.** Follow a coherent existing policy or propose a SemVer-style policy appropriate to the project's release stage. Record the canonical source and agreed rules in existing contributor or agent guidance.
5. **Summarize the result.** Report the chosen version and its evidence, the increment policy, documentation changes, and unresolved compatibility questions.

The agent must wait for the user's choice before creating or changing the canonical `VERSION`. A history-based proposal must explain shipped milestones; commit count alone is not sufficient evidence.

## Default increment policy

These defaults apply when the target project does not already have a coherent documented policy.

| Change | Increment |
| --- | --- |
| Compatible bug fixes, security fixes, or corrections to supported user documentation | Patch |
| New user-visible features, compatible API or configuration additions, or newly supported integrations | Minor |
| Breaking changes at version 1.0 or later | Major |
| Breaking changes before version 1.0 | Define an explicit project rule |
| Unfinished work, tests alone, planning, internal refactors, or changes without a user-visible product or supported-documentation effect | None |

For changes spanning several categories, apply only the largest required increment. Increment when the change is complete and ready for handoff. Do not bump the same change twice or reuse a version for distinct release-worthy product states. Follow the project's existing changelog convention when recording user-visible changes.

## Scope

The skill covers starting-version selection, canonical version authority, and increment policy. It preserves unrelated project instructions and existing version conventions.

Manifest synchronization, lockfile updates, packaging, and release automation require a separate request. Creating tags, commits, packages, publishing artifacts, or releasing the project also requires separate authorization.

## Repository contents

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Skill instructions and workflow |
| [agents/openai.yaml](agents/openai.yaml) | Agent interface metadata, including the display name and short description |
| [README.md](README.md) | Overview, usage examples, and policy summary |

## Maintaining the skill

Keep this README aligned with `SKILL.md`, which defines the skill's behavior. When changing the workflow, preserve the starting-version confirmation step, the distinction between product and dependency versions, and explicit handling of breaking changes before version 1.0. Update the agent metadata if the skill's name or purpose changes.
