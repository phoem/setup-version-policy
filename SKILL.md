---
name: setup-version-policy
description: Set up a project’s canonical VERSION source and define when completed changes require a version increment.
---

# Set Up Version Policy

Use this skill when asked to establish, document, or review how a project chooses its current version and decides when to increment it. It covers starting-version selection and bump policy; it does not set up manifest synchronization, lockfile updates, packaging, or release automation unless separately requested.

## Establish the starting version

Inspect applicable project instructions and existing release/version documentation. Search the repository for version candidates, including a root `VERSION`, package manifests, runtime constants, release notes, and Git tags. Report each candidate with its location and role, distinguishing the declared current product version from dependency or component versions.

Before creating or changing the canonical `VERSION`, ask the user to choose a starting point:

1. Adopt one of the discovered project-version candidates.
2. Review commit history and propose a starting version based on shipped project milestones.
3. Use a version the user supplies.

Ask even when there is only one plausible candidate. Do not initialize or change `VERSION` until the user chooses. While waiting, continue independent read-only review. For a history-based proposal, inspect the actual history and explain the milestone reasoning; do not infer a baseline from commit count alone. Silence is not approval.

Once selected, keep the repository-root `VERSION` file as the single source of truth for the current project version. Other version-bearing files may mirror it, but are not competing authorities. If the repository already has an explicit policy for another canonical location, explain the conflict and ask which authority to keep before changing it.

## Decide when a completed change increments the version

Classify completed changes by their effect on users and compatibility. Follow an existing, documented project policy when it is coherent. If the project has no policy, propose and document a SemVer-style policy suited to its release stage, then ask only when a material choice cannot be resolved from repository evidence.

A useful default is:

- **Patch:** compatible bug fixes, security fixes, and corrections to supported user documentation.
- **Minor:** new user-visible features, compatible API or configuration additions, and newly supported integrations.
- **Breaking:** use the major component after 1.0. For pre-1.0 projects, define how breaking changes are represented instead of assuming post-1.0 SemVer behavior.
- **No increment:** unfinished work, tests alone, planning, internal refactors, or changes with no user-visible product or supported-documentation effect.

For a change spanning multiple categories, apply only the largest required increment. Increment when the change is complete and ready for handoff, not at the start of implementation. Do not bump the same change twice or reuse a version for two distinct release-worthy product states. Follow the repository’s changelog convention when recording the user-visible change.

Document the canonical source and agreed increment rules in the project’s existing contributor or agent guidance. Preserve unrelated instructions and existing version conventions. Do not create tags, commits, packages, publish artifacts, or release the project unless separately authorized.

## Finish

Summarize the chosen starting version and its evidence, the canonical source, the increment policy, the documentation changed, and any unresolved compatibility questions. Do not claim version synchronization or release automation was added unless that work was separately requested and completed.

